# cellagents.dev

Public landing page for the cellagents org. Single static page, no
build step, no runtime. Open `index.html` in a browser to preview.

## Files

- `index.html` - the page
- `style.css` - light palette, flex-centered layout, responsive
- `assets/logo.svg`, `assets/logo.png` - logo variants (SVG preferred,
  PNG as a fallback and favicon)

## Deploy

Any static host works. Simplest options:

- **GitHub Pages**: enable Pages on this repo, source = `main` /
  `root`. DNS: CNAME `cellagents.dev` → `cellagents.github.io`.
- **Caddy on an existing VM**: `root * /srv/cellagents.dev` + `file_server`.

No framework, no dependencies, no secrets.
