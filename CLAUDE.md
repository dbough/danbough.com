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

- `_config.yml` — minimal: `url`, `google_analytics_id` (set to a `G-XXXXXXXXXX` GA4 id to
  switch analytics on; empty string switches it off) and `exclude`.
- `_layouts/default.html` — the only layout. Holds `<head>` (meta, canonical + Open Graph
  tags, favicons, stylesheet links — no web font; the CSS uses a system font stack), the
  GA4 `gtag` snippet inline (guarded
  by `{% if site.google_analytics_id and site.google_analytics_id != "" %}`), and a small
  inline `IntersectionObserver` script before `</body>` that adds `.is-visible` to `.reveal`
  elements (no-ops under `prefers-reduced-motion`). `<body class>` comes from the page's
  `body_class`. No `_includes/`.
- `index.html` — the landing page: front matter + body markup only. A stack of full-width
  sections (`sticky .site-header` → two-column `.hero` (`.hero__text` + framed `.hero__photo`
  with caption, modelled on completelyhumancoaching.com — the `.hero__sub` carries the
  short bio, so there is no separate About section) → `#consulting` with a flex-wrap
  `.offerings` card grid → `#coaching` (`.two-col` who/what lists) → `#projects` (Simply
  Productive Software cards) → `#contact` dark Formspree form → `.site-footer`, LinkedIn
  only). Sections alternate `.section` / `.section--alt` bands and carry `.reveal` for the
  scroll fade. Every off-site link is `target="_blank" rel="noopener"`.
- `404.html` — front matter + a single `.error-page` section. Sets `permalink: /404.html`;
  GitHub Pages serves it for unmatched paths.
- `assets/` — all static assets: `css/` (`normalize.min.css` + hand-written `style.css`, plain
  CSS, no Sass), `img/` (favicons + `dan-bough.png` — a transparent-background headshot shown
  in the hero and About), `fonts/` (icomoon icon font). Referenced by absolute `/assets/...`
  paths. `style.css`'s `@font-face` uses `url('../fonts/icomoon.*')`, which resolves because
  `css/` and `fonts/` are siblings.
- `style.css` — the whole visual system, modelled on dexal.co.uk (system font stack, warm
  paper + white grounds, 4px radius, dark buttons that turn accent on hover, small
  uppercase `.section-label` kickers, `.offerings` cards, dark `.section--dark` contact
  band) with a completelyhumancoaching.com-style framed hero photo. Palette + spacing live
  in `:root` custom properties; `--accent` is dexal's warm `#b96f3a` (the colour of the
  "Start here" label in its contact section). Only the icomoon glyph still used
  (`icon-linkedin`) needs its mapping (`icon-github2` is mapped but currently unreferenced).
- `CNAME` — custom domain (`danbough.com`). Don't remove or rename it.
- The `contact-form` posts to a placeholder `https://formspree.io/f/REPLACE_ME` action —
  swap in the real Formspree endpoint before relying on it.

Front matter keys the layout reads: `layout`, `title` (falls back to "Dan Bough"),
`description`, `body_class`, `permalink`.

## Conventions

- `index.html` and `404.html` share the `style.css` class vocabulary (`section`, `wrapper`,
  `btn`, …). Keep them visually in sync when touching shared structure.
- Icon glyphs come from the icomoon font in `assets/fonts/`; `style.css` maps `icon-github2`
  and `icon-linkedin` to codepoints. Adding an icon means regenerating that font set.
- The sibling site `../completely-human-coaching-com` still uses the older single-card
  structure — the two have diverged; only port structural changes across deliberately.
