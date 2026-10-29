---
name: design-system-options
description: >-
  Reads any project's codebase and brand, then builds one self-contained HTML page proposing a design
  system in three switchable directions (shared foundations; different shape, depth and brand-colour
  placement) with a side-by-side comparison, a sourced-values table, live components and the project's
  own key screen. After the user picks (one direction or a per-layer mix) it produces the approved
  reference: final.html + tokens.css + DESIGN.md. Use when asked for a design system, a visual
  direction, style options, or "show me options to choose from" — e.g. "make a design system for this
  project", "propose design directions", "اعملي ديزاين سيستم للبروجيكت", "نظام تصميم", "خيارات تصميم
  أختار منها", "اعتمد الخيار ب". Not for editing tokens inside existing code (do that after approval) and
  not for critiquing a single screen.
argument-hint: "[scope, e.g. 'customer portal'] | approve <a|b|c|mix description>"
---

# Design-system options → approved reference

`$ARGUMENTS` is either the scope (whole product, or certain roles/screens) or `approve …`.

Two phases. Phase 1 ends with the user choosing; never start phase 2 without that choice.

Files in this skill:
- `assets/template.html`: the page you build from. Its mechanism, demo values and `<!-- FILL -->` markers are explained in `references/page-contract.md`. **Read the contract before writing.** Read the template by section (`grep -n '<section\|═══'`, then `sed -n`). It is large, so don't load it whole.
- `references/approval.md`: phase 2 outputs and how to resolve a mix.

---

## Phase 1: options

### 1. Scope and output folder
- Output folder, first match wins: a location the user names → a deliverables convention in the
  project's or user's CLAUDE.md / rules → `./design-system/<YYYY-MM-DD>/` at the project root.
  Everything goes in that folder: `index.html` plus `assets/` (fonts and logo only).
- If the scope is ambiguous (which app, which roles, admin included?), ask **one** AskUserQuestion.
  For everything else, assume and list the assumption under "Open decisions".

### 2. Understand the project from evidence, not memory
Collect each item with `file:line` or the command that showed it:
- **Stack**: `package.json`, `composer.json`, `Gemfile`, etc.; UI framework (Tailwind v3/v4,
  Bootstrap, MUI, custom SCSS…); where styles live.
- **Current visual language**: CSS custom properties, SCSS/LESS variables, `tailwind.config` /
  `@theme`. Also count frequencies with grep: hex colours, `border-radius`, `font-size`, `box-shadow`,
  component heights. The most frequent values are the de-facto system; the long tail is the mess
  the new system fixes. Note it with numbers ("11 font sizes between 11 and 34px").
- **Brand**: logo SVG/PNG, brand guide (PDF, images, a `brand/` folder), fonts in the repo. Search
  the repo and its parent folder before saying "none". No brand found → ask the user once for a
  guide or primary colours. If they have none, derive the palette from the current UI.
- **Language and direction**: `lang`/`dir`, i18n files. Arabic, Hebrew, Persian or Urdu means
  RTL-first: logical properties, mirrored directional icons, a minimum size for script legibility.
- **Key screen**: the most-used screen in scope (dashboard/home route, most-linked page). Use its
  real labels and realistic data later.
- **Constraints**: a mandated standard (government design system, accessibility law, a parent
  company's design system), a font licence, a dark-mode requirement.
- Optional helpers: if skills for tokens, RTL/bilingual UI, a national design standard or general
  UI critique are installed, load the relevant ones. Never depend on them.

### 3. Shared foundations (identical in all three options)
Brand ramps (100/80/60/40/20% + an 8% wash) · one neutral family · status colours (wash · ink
≥ 4.5:1 on its wash · dot) · type scale (16px body; headings in steps) · 4px spacing grid ·
component heights S/M/L = 32/40/48, growing on touch screens to 40/48/56 · one easing curve and
duration. **Every number has a source** (project file, brand guide, named standard or public token
set) and a status: *measured* (you opened it today) or *to confirm*. Compute every contrast ratio
and write none from memory.

### 4. Three directions
They differ on **three axes only**: shape (radii), depth (shadow vs hairline), and where the brand
colour lives (primary action, app shell/header, categories). Default framing, adapted to the
evidence:

| | Spirit | Built from |
|---|---|---|
| **A** | Evolution of what exists: lowest migration cost | the current UI, put on a scale |
| **B** | Institutional / standard: most systematic | the applicable standard or the framework's own conventions |
| **C** | Brand-forward: most distinctive | strongest brand colour on the shell and primary action, secondary palette for categories |

Each option gets a one-word name, one paragraph, and facts: primary button colour with its
contrast, radii, depth, migration cost. All three must be shippable; none is a strawman.

### 5. Build
Copy `assets/template.html` to `<out>/index.html` and follow the contract:
1. Set `lang`/`dir`, title, THESIS/SOURCES comment, fonts (project font files into `assets/` only
   if they are in the repo or brand guide; otherwise Google Fonts), and the logo.
2. Replace layer 1 with the foundations, and layer 2 with the three options' tokens.
3. Fill every `<!-- FILL: … -->` with project content in the project's UI language, then delete the
   marker. Remove components the project doesn't use and add ones it does (e.g. a data grid or
   a stepper).
4. Rebuild `#screen` as the project's key screen.
5. `grep -c 'FILL:\|DEMO' index.html` must be 0. No demo colour may survive.

### 6. Verify before showing
- Screenshot `?o=a`, `?o=b`, `?o=c` at 1440 and 390 px (headless Chrome or a browser MCP) and look
  at them. Switching must change the bar, cards, buttons, table and sample screen.
- No horizontal scroll at 390. No console errors. Directional icons flip in RTL.
- Every contrast ratio on the page is computed.

### 7. Present and ask
- A short reply: the `file://` link, one line per option, and the top two open decisions.
- Then AskUserQuestion: "Which direction?" with options A / B / C / Mix. If the answer is Mix,
  ask per layer: surfaces & depth, shape, colour roles & primary action, header/nav bar, focus ring,
  alerts. Ask the open decisions from `#decide` in the same round, up to 4 questions per call.
- Do not publish, upload, commit or push anything unless the user explicitly asks.

## Phase 2: approval
Triggered by the user's choice (from step 7, or `approve …` later). Follow
`references/approval.md`: it writes `final.html`, `tokens.css` and `DESIGN.md` next to
`index.html`, leaves `index.html` untouched, and verifies them. Finish by offering, without doing
it, the next step: wiring `tokens.css` into the codebase and adding a one-line pointer to
`DESIGN.md` in the project's CLAUDE.md so future design work follows it.
