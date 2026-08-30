# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Personal website for **danbough.com**, a small Jekyll site served by **GitHub Pages** from the
`main` branch. No npm, no bundler — Jekyll (via GitHub Pages' built-in build) is the only build
step. Pushing to `main` deploys.

The structure mirrors Dan's other GitHub Pages Jekyll site,
`../completely-human-coaching-com` — keep the two consistent when making structural changes.

## Local preview

Pages use a Jekyll layout, so opening the HTML with `file://` won't render correctly. Use a
globally-installed Jekyll (already on PATH):

```
jekyll serve        # http://localhost:4000
jekyll build        # writes _site/
```

There is intentionally no `Gemfile`.

## Architecture

- `_config.yml` — minimal: `google_analytics_id` (empty placeholder; set to a `G-XXXXXXXXXX`
  GA4 id to switch analytics on) and `exclude`.
- `_layouts/default.html` — the only layout. Holds `<head>` (meta, favicons, Lato web font,
  stylesheet links) and the GA4 `gtag` snippet inline, guarded by
  `{% if site.google_analytics_id and site.google_analytics_id != "" %}`. `<body class>` comes
  from the page's `body_class`. No `_includes/`.
- `index.html`, `404.html` — front matter + body markup only. The `.page-container` /
  `.landing` / `.wrapper` chrome lives in each page body (it differs per page), not in the
  layout. `404.html` sets `permalink: /404.html`; GitHub Pages serves it for unmatched paths.
- `assets/` — all static assets: `css/` (`normalize.min.css` + hand-written `style.css`, plain
  CSS, no Sass), `img/` (favicons), `fonts/` (icomoon icon font). Referenced by absolute
  `/assets/...` paths. `style.css`'s `@font-face` uses `url('../fonts/icomoon.*')`, which
  resolves because `css/` and `fonts/` are siblings.
- `CNAME` — custom domain (`danbough.com`). Don't remove or rename it.

Front matter keys the layout reads: `layout`, `title` (falls back to "Dan Bough"),
`description`, `body_class`, `permalink`.

## Conventions

- `index.html` and `404.html` share the same CSS class vocabulary (`page-container`, `landing`,
  `wrapper`, `text-white`, `bg-primary-color`, …). Keep them visually in sync when touching
  shared structure.
- Icon glyphs come from the icomoon font in `assets/fonts/`; `style.css` maps `icon-github2`,
  `icon-linkedin`, `icon-pencil`, etc. to codepoints. Adding an icon means regenerating that
  font set.
