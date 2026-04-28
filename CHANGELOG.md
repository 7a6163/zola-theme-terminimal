# Changelog

All notable changes to this theme will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [3.2.1] - 2026-04-28

Branding switch: this fork now points at itself rather than upstream
`pawroman/zola-theme-terminimal` for install instructions, demo URL,
build badge, and authorship metadata. Upstream is still credited via the
new `[upstream]` block in `theme.toml`, the dual-credit footer, the
preserved MIT License attribution, and the changelog history note.

### Changed

- `theme.toml`: `homepage` and `demo` now point at the fork.
  `[author]` is the fork maintainer (Zac / 7a6163). New `[upstream]`
  block credits Paweł Romanowski's repo as the immediate upstream.
  The pre-existing `[original]` block (Radek Kozieł's Hugo theme) is
  preserved.
- `README.md`: build-status badge, live-demo URL, `git clone` /
  `git submodule` install snippets, and the "How to contribute" PR
  link all switch to the fork.
- `config.toml`: demo `base_url` and the demo navigation menu's GitHub
  link now point at the fork.
- `templates/index.html` (default footer): now reads
  *"Theme: Terminimal by pawroman, fork by 7a6163"* — both linked.
  Sites that override `config.extra.copyright_html` are unaffected.
- `CLAUDE.md`: demo URL in the project description updated.

### Preserved

- `README.md` MIT License attribution to Paweł Romanowski (legal).
- `CHANGELOG.md` v3.0.0 entry's "fork from pawroman v2.0.0" history
  note.

## [3.2.0] - 2026-04-22

Accessibility and micro-SEO polish, all without adding JavaScript.

### Added

- **`<link rel="canonical">`** on every rendered page (post, section,
  taxonomy, term, and 404). Prevents duplicate-content confusion for search
  engines when URLs can be reached via multiple paths.
- **Skip-to-content link** (`<a class="skip-link" href="#main">`) rendered
  as the first element in `<body>`. Visually hidden until it receives
  keyboard focus; lets keyboard and screen-reader users bypass the header.
- **`<main id="main">`** landmark anchor target for the skip link.
- **`aria-label="Main"`** on the site `<nav class="menu">`.
- **`aria-current="page"`** on the active menu item's `<a>` (in addition
  to the existing `class="active"` colour change, which was invisible to
  screen readers).
- **`rel="tag"`** on taxonomy `<a class="post-tag">` links (microformat).

### Changed

- **Post dates now use `<time datetime="..." class="post-date">`** instead
  of `<span>`. Provides a machine-readable ISO 8601 timestamp alongside
  the human-facing `YYYY-MM-DD` display. Existing `.post-date` CSS
  selectors continue to match.

### Removed

- **`<meta name="robots" content="noodp">`** — this directive has been
  ignored by Google (2017) and Bing (2019). Removing clears the dead line.

## [3.1.0] - 2026-04-22

SEO refinements on top of v3.0.0.

### Changed

- **`og:locale` now auto-derives** from `config.default_language` (with
  dashes replaced by underscores, e.g. `zh-TW` → `zh_TW`) when
  `config.extra.og_locale` is unset. Previously the tag was simply omitted
  when no explicit value was set. `og_locale` still overrides for sites
  whose `default_language` lacks a region (e.g. `"en"` → set
  `og_locale = "en_US"`).
- **`page_titles` default flipped from `"main_only"` to `"combined"`.**
  Most blogs expect `"Page title | Main title"` for SEO and share previews.
  Sites that had `page_titles = "main_only"` set explicitly are unaffected;
  sites relying on the old implicit default will see their `<title>`
  strings change to the combined form. The demo `config.toml` now leaves
  the key commented out to exercise the new default.

## [3.0.0] - 2026-04-22

Major bump from the upstream
[pawroman/zola-theme-terminimal](https://github.com/pawroman/zola-theme-terminimal)
v2.0.0 baseline, reflecting breaking changes in the required Zola version and
in the rendered HTML element hierarchy. Also adds SEO and performance
improvements.

### Added

- **OpenGraph completion.** `og:site_name` on every page, optional `og:locale`
  via `config.extra.og_locale`. On post pages:
  `article:published_time`, `article:modified_time`,
  `article:author` (from `config.extra.author`),
  and one `article:tag` per taxonomy tag.
- **JSON-LD structured data.** `BlogPosting` on articles, `WebSite` on the
  home and sections. Taxonomy, tag, and 404 pages are skipped.
  Emitted via a new `structured_data` block in `index.html`, so downstream
  users can override.
- **`config.extra.author_url`** — optional author homepage URL used in
  JSON-LD as `author.url` for `BlogPosting` and `WebSite`.
- **`config.extra.og_locale`** — optional OpenGraph locale (BCP 47 with
  region, e.g. `"en_US"`, `"zh_TW"`). When unset, no `og:locale` is emitted.
- **Font preload.** `<link rel="preload">` for the primary
  `hack-regular.woff2`, declared before stylesheet links for parallel fetch.
- **`font-display: swap`** on every `@font-face` rule in
  `sass/font-hack.scss` and `sass/font-hack-subset.scss`.

### Changed

- **Semantic HTML5 landmarks.** Content wrappers updated:
  - `<div class="content">` → `<main class="content">`
  - `<div class="post">` / `<div class="post on-list">` → `<article class="...">`
  - `<div class="pagination">` → `<nav class="pagination" aria-label="...">`

  All CSS class names are preserved; existing selectors continue to match.
- **Modern charset declaration.** `<meta http-equiv="content-type" ...>`
  replaced by `<meta charset="utf-8">` as the first element in `<head>`.
- **Zola 0.22+ `[markdown]` config syntax.** Flat
  `highlight_code` / `highlight_theme` keys replaced with the nested
  `[markdown.highlighting]` table. Default highlight theme changed from
  `boron` (removed in newer Zola bundles) to `catppuccin-mocha`.

### Compatibility

- Tested with **Zola 0.22.1**. Earlier Zola versions (≤ 0.21.x) will not
  build due to the `[markdown]` syntax update.
- The switch to HTML5 landmark tags is a potentially breaking change for
  downstream CSS that relies on tag-name selectors such as
  `div.post` or `div.content` — use the class selectors instead
  (`.post`, `.content`).

[Unreleased]: https://github.com/7a6163/zola-theme-terminimal/compare/v3.2.1...HEAD
[3.2.1]: https://github.com/7a6163/zola-theme-terminimal/releases/tag/v3.2.1
[3.2.0]: https://github.com/7a6163/zola-theme-terminimal/releases/tag/v3.2.0
[3.1.0]: https://github.com/7a6163/zola-theme-terminimal/releases/tag/v3.1.0
[3.0.0]: https://github.com/7a6163/zola-theme-terminimal/releases/tag/v3.0.0
