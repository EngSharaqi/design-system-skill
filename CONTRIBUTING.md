# Contributing

Thanks for helping improve **design-system-options**.

## How changes get in

- `main` is protected. Nobody pushes to it directly.
- Every change arrives as a pull request and merges only after the maintainer reviews and
  approves it (see `.github/CODEOWNERS`).
- For anything bigger than a typo, open an issue first to agree on the change.

## What a good pull request includes

- **What and why.** One paragraph: the problem, and how the change fixes it.
- **Proof.** If you changed `assets/template.html`, attach screenshots of options A, B and C at
  1440px and 390px, and confirm:
  - no horizontal scroll at 390px;
  - no console errors;
  - every contrast ratio written on the page is computed, not typed in;
  - the page still reads correctly with `dir="rtl"`.
- **Neutrality.** The template must stay brand-neutral: no real company names, logos, palettes
  or client content.
- **Docs.** If behaviour changes, update `SKILL.md`, the files in `references/` and the README.
  Add a line to `CHANGELOG.md`.

## Versioning

Releases follow [semantic versioning](https://semver.org): a breaking change to the workflow or
the output files is a major version, a new capability is a minor version, and a fix is a patch.
