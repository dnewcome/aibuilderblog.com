# AI Builder Blog

A static blog built with [Eleventy](https://www.11ty.dev/) (11ty). Dark, editorial design. No frameworks, no preprocessors — just Nunjucks templates, plain CSS, and Markdown.

Topics: AI, energy, software, and the future of building technology.

## Technology

| Layer | Tool |
|---|---|
| Static site generator | [Eleventy 3.x](https://www.11ty.dev/) |
| Templating | [Nunjucks](https://mozilla.github.io/nunjucks/) |
| Styles | Plain CSS with custom properties |
| Content | Markdown with YAML frontmatter |
| Hosting | DigitalOcean App Platform (static site) |
| Runtime | Node.js ≥ 18 (build only — no server in production) |

## Local development

```bash
npm install
npm run serve
```

Opens at `http://localhost:8080`. The dev server reloads on file changes.

> **Note:** Open via the dev server, not by double-clicking `_site/index.html`. Root-relative CSS paths (`/css/style.css`) require a server to resolve correctly.

Other commands:

```bash
npm run build   # one-off build → _site/
npm run watch   # build + watch, no browser reload
```

## Publishing a new article

1. Create a Markdown file in `src/blog/posts/`:

   ```
   src/blog/posts/your-post-slug.md
   ```

2. Add YAML frontmatter at the top:

   ```yaml
   ---
   title: "Your Post Title"
   date: 2026-03-15
   description: "A one-sentence summary shown in post listings and meta tags."
   tags: ["posts", "ai"]
   ---
   ```

   - `title` — required
   - `date` — required; use `YYYY-MM-DD`. Posts are sorted newest-first by this field.
   - `description` — optional but recommended; used as the excerpt in listings
   - `tags` — always include `"posts"` (needed for collection); add topic tags freely

3. Write the body in Markdown below the frontmatter. Standard Markdown is supported: headings, bold, italics, lists, blockquotes, code blocks, links, images.

4. Run `npm run serve` and visit `http://localhost:8080/blog/posts/your-post-slug/` to preview.

### Available tags

No fixed taxonomy — use whatever makes sense. Current tags in use: `ai`, `programming`, `energy`, `meta`.

### Adding images

Drop image files into `src/images/` and reference them with an absolute path:

```markdown
![Alt text](/images/your-image.png)
```

Using an absolute path ensures the image loads correctly regardless of the post's URL depth.

## Project structure

```
aibuilderblog.com/
├── .eleventy.js          # Eleventy config: collections, filters, passthrough copy
├── package.json
├── .gitignore
├── .do/
│   └── app.yaml          # DigitalOcean App Platform deploy spec
└── src/
    ├── _includes/
    │   ├── base.njk       # Page shell: sticky header, nav, footer
    │   └── post.njk       # Single post layout (wraps base.njk)
    ├── css/
    │   └── style.css      # All styles — dark theme, indigo accent, responsive
    ├── images/            # Static images referenced in posts and pages
    ├── files/             # Other static files (PDFs, downloads)
    ├── index.njk          # Homepage: hero + recent posts grid
    └── blog/
        ├── index.njk      # Paginated post listing (10 posts/page)
        └── posts/
            ├── posts.json # Applies layout and "posts" tag to all .md files here
            └── *.md       # Blog posts
```

The build outputs to `_site/` (git-ignored). DigitalOcean runs the build on every push.

## Deployment

The site deploys automatically via DigitalOcean App Platform on every push to `main`.

**First-time setup:**

1. Update `YOUR_GITHUB_USERNAME` in `.do/app.yaml`
2. Push the repo to GitHub
3. Create the app from the spec:
   ```bash
   doctl apps create --spec .do/app.yaml
   ```
   Or create it in the DO dashboard — make sure to select **Static Site** as the component type.

**Every subsequent deploy:**

```bash
git add src/blog/posts/your-post.md
git commit -m "add: your post title"
git push
```

DigitalOcean detects the push, runs `npm run build`, and serves `_site/`.

> Do not commit `_site/` — it is git-ignored and rebuilt by DO on every deploy.
