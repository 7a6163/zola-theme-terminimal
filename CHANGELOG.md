# Changelog

All notable changes to this theme will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

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

[Unreleased]: https://github.com/7a6163/zola-theme-terminimal/compare/v3.1.0...HEAD
[3.1.0]: https://github.com/7a6163/zola-theme-terminimal/releases/tag/v3.1.0
[3.0.0]: https://github.com/7a6163/zola-theme-terminimal/releases/tag/v3.0.0
