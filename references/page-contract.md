# Page contract: `index.html` (the options page)

The page is one self-contained HTML file that opens from `file://` with no server. Next to it,
`assets/` holds only the fonts and the logo. The only external requests allowed are Google Fonts.

## Template anatomy (`assets/template.html`)

Find each part with `grep -n '═══\|<section\|<header\|<script' assets/template.html`.

| Part | What it is | What you do |
|---|---|---|
| `<html lang dir data-opt="a" data-font>` + THESIS/SOURCES comment | page identity | set lang/dir/title; rewrite the comment |
| CSS layer 1 `:root` | brand ramps, neutrals, status, type, space, heights, motion, mobile scale (**DEMO values**) | replace with the project's foundations |
| CSS layer 2 `[data-opt="a|b|c"]` | semantic roles per option | set the three directions |
| CSS layers 3–5 | page shell, specimens, components, sample screen, phone frame | keep; adjust `[data-opt]` component rules |
| `header.top` | `#opt-seg` option switcher, `#font-seg` font switcher | relabel |
| `nav.toc` | table of contents | match the sections you keep |
| `main > section.sec` | the sections below | fill |
| `<script>` | ramps, type scale, spacing, breakpoints, icons, roles, switching | update the arrays only |

## Mechanism: do not break it

- Everything that differs between options is a **token** in `[data-opt="x"] { … }`. Anything a token
  can't express is a `[data-opt="x"] .component { … }` rule placed next to that component. Components
  never hard-code a colour or a radius.
- Minimum role set per option: `--ground --band --surface --sunken --ink --ink-2 --ink-3 --line
  --line-input --primary --primary-hover --on-primary --accent --accent-wash --select-wash
  --select-ink --btn2-bg --btn2-ink --btn2-border --btn2-hover --link --focus --r-card --r-inner
  --r-ctl --r-btn --r-chip --card-border --sh-1 --sh-2 --sh-pop --card-pad --bar-bg --bar-ink
  --bar-ink-2 --bar-line --bar-active --bar-active-ink --field-bg`.
- Any `[data-set-opt="x"]` element switches the option. `.opt` cards use `aria-pressed` and
  switcher tabs use `aria-selected`. The `#cmp [data-col]` cells of the shown option are
  highlighted. `#roles` is redrawn from computed values, and `[data-tok]` spans print live token
  values.
- `?o=b` opens an option and `?only=c-card` shows a single section (for screenshots). The choice
  persists in `localStorage` (`ds-opt`, `ds-font`) inside try/catch.

## Sections, in order

1. **`#choose`**: three options for one system. A lead paragraph says what the options share and
   the three things they differ on. Then three cards, each with a mini sketch, a name, a paragraph
   and `dl.opt__facts`. Then a comparison table with ≥ 8 rows: page ground, header bar, primary
   button + contrast, secondary button, accent use, secondary palette use, radii, surface
   separation, closeness to the applicable standard, impact on the rest of the product.
2. **`#sources`**: what we took · value · source · status (`verify--yes` measured /
   `verify--q` to confirm), plus one paragraph on the limits of the sources.
3. **`#color`**: primary and secondary ramps, status colours, and the roles table for the shown
   option.
4. **`#type`**: the live scale (it changes on mobile), the weights, and one paragraph at real
   length in the project's language.
5. **`#space`**: spacing scale, breakpoints with "you are here", component heights.
6. **`#shape`**: radii, shadows, motion.
7. **`#icons`**: the icon set and the focus ring.
8. **`#components`**: buttons (types, sizes, states including disabled/loading), fields
   (default/error/success/disabled/help), choice (select, multi-select, checkbox, radio, switch,
   choice cards), search, tabs and segmented control, tags and status chips, avatar, breadcrumb and
   pagination, cards, table, alerts / modal / menu, nav bar. Drop what the project doesn't use; add
   what it does.
9. **`#screen`**: the project's key screen with real labels and realistic data, on desktop and in
   the 390px phone frame.
10. **`#decide`**: open decisions. The first is always which option, noting that a mix is allowed
    with one source per layer. Then every assumption you couldn't settle: font licence, mandated
    standard, base text size, error colour, dark mode…

## Quality bar

- Logical properties only (`margin-inline`, `inset-inline-start`, `text-align: start`). Directional
  icons carry `.flip`. The page must read correctly in both `dir="ltr"` and `dir="rtl"`.
- Text contrast ≥ 4.5:1 and UI/focus contrast ≥ 3:1, computed with the page's `ratio()` helper or a
  script.
- `font-variant-numeric: tabular-nums` in tables. Numerals follow the project's locale.
- Inputs ≥ 16px on mobile so iOS doesn't zoom. Include `prefers-reduced-motion`.
- Copy is in the project's UI language, written for the person deciding: short sentences, and
  every number with its unit and source.
