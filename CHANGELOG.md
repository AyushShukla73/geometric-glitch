# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Comprehensive documentation suite (README, CONTRIBUTING, CODE_OF_CONDUCT, SECURITY, CHANGELOG)
- GitHub repository metadata (.github/repository_description.txt, .github/repository_topics.txt)
- MIT License file

### Changed
- Updated .gitignore to exclude local GitHub metadata files

## [1.2.0] - 2024-11-12

### Added
- Cache-control meta tags (`no-cache`, `no-store`, `must-revalidate`) to force browser revalidation on GitHub Pages
- `.gitignore` with rules for local config, OS files, and GitHub metadata

### Fixed
- Removed "claude" from GitHub contributors list by rewriting commit history (stripped Co-Authored-By trailers)

### Changed
- Force-pushed cleaned history to GitHub Pages

## [1.1.0] - 2024-11-06

### Added
- **Editorial/Brutalist redesign** with distinctive visual identity:
  - Typography: Syne (display) + JetBrains Mono (UI/mono) via Google Fonts
  - Tight accent palette: `#e8ff00` (yellow) on zinc/neutral backgrounds
  - Dark/light mode via `prefers-color-scheme` CSS media query
  - Asymmetric upload card (content + drop zone side panel)
  - Tabbed results panels (4 stages: Mirror Grid, Vertical, Horizontal, Final)
  - Compact responsive canvas grid (`auto-fit`, `minmax(260px, 1fr)`)
  - Pipeline progress indicator with animated step markers
  - Purposeful micro-interactions: reveal animations, shimmer progress bar, scanline top bar, pulse glow on hover, spin loader
  - Realistic copy: "Glitch Lab", "Geometric image processor — mirror, slice, recombine"
- **Accessibility enhancements**:
  - Full keyboard navigation (tabindex, role="button", Enter/Space activation)
  - ARIA live regions (polite for results, assertive for loader)
  - ARIA tablist/tab/tabpanel for tabbed interface
  - Visible focus styles (`:focus-visible`)
  - `prefers-reduced-motion` support (disables all animations/transitions)
  - WCAG AA contrast in both themes
  - Semantic HTML structure (proper heading hierarchy, landmarks)
- **Screenshots folder** with 7 pipeline step images for documentation
- **README screenshots section** with organized table linking all pipeline stages

### Changed
- Restructured HTML with clear semantic sections (header, pipeline nav, upload, results tabs, footer)
- Improved drag-and-drop UX (entire card clickable, visual feedback on dragover/focus)
- Progress indicator now shows step labels and percentages
- Final download button prominently styled with accent color and hover effects

### Fixed
- File input click handler (was failing on hidden input; fixed by attaching to entire card)
- Variable name collision in strip recombination loops (`tog` → `togH` for horizontal)

## [1.0.0] - 2024-11-05

### Added
- **Core 4-step geometric glitch pipeline**:
  1. **2×2 Mirror Grid** — Canvas transformations using `translate()` + `scale(-1,1)`, `scale(1,-1)`, `scale(-1,-1)`
  2. **Vertical Strip Recombination** — 40px strips, alternating left/right edges inward (two-pointer algorithm)
  3. **Horizontal Strip Recombination** — 40px strips, alternating top/bottom edges inward (transposed algorithm)
  4. **PNG Export** — `canvas.toDataURL('image/png')` with download link
- **Upload handling** — Drag-and-drop, click-to-browse, FileReader API, 50MB limit
- **Progressive rendering** — Loading overlay, progress bar, pipeline step indicators
- **Intermediate previews** — Individual quadrant canvases, split views, recombined views, joined results
- **GitHub Pages deployment** — Served from `master` branch root
- **Initial README** with pipeline documentation and usage instructions

### Technical Details
- Single-file architecture: `index.html` (HTML + CSS + JS)
- Zero external JavaScript dependencies
- Vanilla ES2020+ JavaScript
- Canvas 2D API for all image processing
- Google Fonts for typography (loaded via `<link rel="preconnect">`)

---

## Versioning Notes

- **Major** — Breaking changes to pipeline behavior or API
- **Minor** — New features, significant UI/UX changes, accessibility improvements
- **Patch** — Bug fixes, documentation updates, minor styling tweaks

This project uses a single `master` branch deployed directly to GitHub Pages. No separate release branches.