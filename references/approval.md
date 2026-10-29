# Phase 2: the approved reference

Input: the user's choice (one option, or a source option per layer) and their answers to the open
decisions. Output: three files next to `index.html`. `index.html` stays as the record of what was
proposed.

## Resolving the choice

Build a decision table first; everything else is generated from it:

| Layer | Tokens / rules it covers | Source |
|---|---|---|
| Surfaces & depth | `--ground --band --surface --sunken --line --card-border --sh-* --card-pad` | A/B/C |
| Shape | `--r-*` | A/B/C |
| Colour roles | `--ink* --primary* --on-primary --accent* --select-* --btn2-* --link` | A/B/C |
| Header / nav bar | `--bar-*` + `[data-opt] .appbar…` rules | A/B/C |
| Focus | `--focus` | A/B/C |
| Alerts, tables, other component rules | the `[data-opt="x"] .component` rules | A/B/C |
| Open decisions | font, base size, error colour, standard in scope… | user decision / standard |

Rules:
- One source per layer. Never blend values from two options inside the same layer.
- For each layer, copy that option's token values into `:root`. For component rules, keep the
  chosen option's `[data-opt="x"] …` rules with the prefix stripped, and delete the other options'
  rules for that component.
- Mixing layers can break contrast (e.g. B's bar with C's focus ring), so recompute every pair
  that changed hands.

## 1. `final.html`, the visual reference
Start from `index.html`:
- Remove `data-opt` from `<html>`, the option switcher, the `#choose` section and the comparison
  table. Remove the font switcher if the font is decided.
- Insert `#decisions` ("Approved decisions") as the first section. It holds a lead paragraph with
  the date and a one-sentence summary ("Mostly A; header and focus ring from B; error colour from
  <standard>"), then a table: Layer · Decision (value + computed contrast) · Source.
- Keep sources, foundations, components and the sample screen, all now rendering the approved
  system only. Unresolved decisions move to a short "Still open" list at the end.
- Update the THESIS comment and add "Approved <date>" to the `<title>`.
- Check: `grep -c 'data-opt' final.html` is 0, and screenshots at 1440 and 390 look right.

## 2. `tokens.css`, the machine reference
Only `:root` custom properties: brand ramps, neutrals, status, type, space, heights, radii, shadows,
motion, then the resolved semantic roles, plus the mobile `@media` block. Group them under comments
(`/* Brand */`, `/* Semantic */`, …). Note the source of each non-obvious value in a trailing
comment. No selectors other than `:root` and the media query. This is the file a developer imports
or maps onto Tailwind `@theme` / SCSS variables.

## 3. `DESIGN.md`, the rules for whoever designs next (people or AI agents)
Keep it short and imperative, in the project's working language:
1. **Status**: approved on <date> by <user>, scope, with links to `final.html` and `tokens.css`.
2. **Decisions**: the decision table.
3. **Tokens**: a table of the semantic roles (token · value · use for). Then the rule: "use semantic
   tokens in components; brand ramps only inside tokens.css".
4. **Type, space, sizes**: the scales and when to use each step.
5. **Components**: for each kept component, give its variants, sizes, states and 1–3 usage rules
   ("one primary button per region", "label above the field, never inside").
6. **Do / Don't**: 6–10 lines drawn from this project's evidence (e.g. "no hard-coded hex; the old
   UI had 47").
7. **Still open**: the decisions not yet made.

Then tell the user what was written, and offer two next steps without doing them: wiring
`tokens.css` into the codebase, and adding to the project's CLAUDE.md a pointer like "UI work
follows design-system/<date>/DESIGN.md".
