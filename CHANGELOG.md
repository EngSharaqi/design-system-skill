# Changelog

## v1.0.0 (2026-10-29)

First release.

- **Phase 1:** reads a project's stack, styles, brand, language and key screen, then builds
  `index.html` with three switchable directions (A · evolution, B · institutional,
  C · brand-forward). The page includes a comparison table, a sourced-values table, foundations,
  a full component sheet, the project's own screen on desktop and phone, and open decisions.
- **Phase 2:** once the user picks an option or a per-layer mix, writes the approved reference:
  `final.html`, `tokens.css` and `DESIGN.md`.
- A neutral `assets/template.html`, with a live demo on GitHub Pages.
- Built-in checks: computed contrast, no horizontal scroll at 390px, LTR and RTL, no leftover
  demo values or FILL markers.
