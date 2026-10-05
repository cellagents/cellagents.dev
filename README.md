# cellagents.dev

Public website for the Cell agents project at
[`cellagents.dev`](https://cellagents.dev), Jekyll on GitHub Pages,
autodeploys from `main`. Covers landing, developer, educator and
MCP-connection pages.

## Pages

- `/` - general-public landing (hero + buttons)
- `/developers/` - repository map and architecture diagrams
- `/educators/` - lesson plan and learning outcomes
- `/about/` - project summary and contact
- `/play-via-ai/` - how to connect external MCP clients

## Files

- `_config.yml` - site title, tagline fragments, link targets
- `_layouts/default.html` - the one layout, with a `layout_mode: hero`
  switch used by the homepage
- `_includes/hero.html`, `_includes/mermaid.html` - partials
- `index.md`, `about.md`, `developers.md`, `educators.md`, `play-via-ai.md` - page content
- `style.css` - shared styles
- `assets/logo.svg`, `assets/logo.png`
- `CNAME` - apex domain `cellagents.dev`

## Editing

- Change tagline → `_config.yml`
- Change nav / button labels or targets → `_config.yml` (`links:` block)
- Add a page → new `.md` with front matter (`layout: default`,
  optional `title:` and `permalink:`)

## Local preview

```bash
sudo apt install ruby-full build-essential
gem install --user-install bundler
cd cellagents.dev
bundle install
bundle exec jekyll serve
# http://127.0.0.1:4000
```

Not strictly needed: GitHub Pages builds on push.

## Deploy

Automatic from `main`. Status at
<https://github.com/cellagents/cellagents.dev/actions> under
`pages-build-deployment`.
