# design-system-options

A [Claude Code](https://claude.com/claude-code) skill. It reads your project and proposes a design
system **as three directions you can switch between on one page**. Once you pick one (or a mix),
it writes the **approved reference** that all later design work follows.

```
"make a design system for this project"
        │
        ▼
 ┌──────────────────────────────┐      you choose       ┌───────────────────────────┐
 │ index.html: 3 options         │ ───────────────────▶ │ final.html: approved      │
 │ A · B · C, one click to swap  │  A / B / C / mix     │ tokens.css: the variables │
 │ comparison · sources · screen │                      │ DESIGN.md: rules to follow│
 └──────────────────────────────┘                       └───────────────────────────┘
```

> نسخة عربية مختصرة في [آخر الصفحة](#بالعربي).

---

## Why

Asking for "a design system" usually gets you one opinionated mock-up, and you can only say yes or
no to it. This skill gives you a decision instead:

- **Three real alternatives built on the same foundations.** You compare the choices that matter
  (shape, depth, where the brand colour goes), not three random styles.
- **Grounded in your code.** Colours, radii, font sizes and shadows are counted from your
  stylesheets. The options start from what you have, and every number cites its source
  (`file:line`, brand guide or standard).
- **Shown on your own screen.** The page renders your most-used screen with your real labels in
  every option, alongside a full component sheet.
- **It ends with a reference, not a mood board.** After you decide, you get a single approved page,
  a `tokens.css` and a `DESIGN.md` that people and AI agents can follow from then on.

## What you get

### Phase 1: `index.html`, the options page

One self-contained HTML file (opens from disk, no server, no build). A switcher in the header
restyles the **whole page** between options A, B and C:

| Section | Contents |
|---|---|
| **Three options** | One card per direction (sketch, description, key facts) and a row-by-row comparison table |
| **Sources** | Every value we used, where it came from, and whether it was *measured* or is *to confirm* |
| **Colour** | Brand and secondary ramps, status colours (wash · ink · dot, contrast computed), semantic roles of the shown option |
| **Type** | The full scale with live values (it changes on mobile), weights, a real-length paragraph |
| **Space & grid** | 4px spacing scale, breakpoints with "you are here", S/M/L component heights |
| **Shape, depth, motion** | Radii, shadow levels, easing |
| **Icons & focus** | Icon set, a focus ring that passes contrast |
| **Components** | Buttons, fields, selects, checkboxes/radios/switches, search, tabs, tags/status, avatar, breadcrumb/pagination, cards, table, alerts/modal/menu, nav bar, each in all its states |
| **Sample screen** | Your key screen, on desktop and in a 390px phone frame |
| **Open decisions** | The questions only you can answer (font licence, mandated standard, base size…) |

The three directions share the same foundations (palette, type scale, spacing, sizes) and differ
on exactly three axes:

| | Direction | Typical shape |
|---|---|---|
| **A** | Evolution of your current UI: lowest migration cost | your existing radii and depth, put on a consistent scale |
| **B** | Institutional / standard: most systematic | hairlines instead of shadows, tighter radii, follows the applicable standard |
| **C** | Brand-forward: most distinctive | brand colour on the shell and primary action, secondary palette for categories |

### Phase 2: the approved reference

After you choose, in the same folder:

| File | For | Contents |
|---|---|---|
| `final.html` | visual reference | The same page rendering **only** the approved system, opening with an *Approved decisions* table (layer · decision · source) |
| `tokens.css` | developers | `:root` custom properties: brand ramps, neutrals, status, type, space, sizes, radii, shadows, motion, resolved semantic roles, mobile scale |
| `DESIGN.md` | whoever designs next, people or AI | Decisions, token roles and when to use each, scales, per-component rules, do / don't, what's still open |

`index.html` stays untouched as the record of what was proposed.

You don't have to take one option whole. Mix them **per layer**, for example *"surfaces and shape
from A, header bar and focus ring from B"*. The skill takes each layer from one option only, so the
result stays coherent, and it recomputes contrast for every pair that changed.

## Install

Clone into your personal skills folder:

```bash
git clone https://github.com/EngSharaqi/design-system-skill.git ~/.claude/skills/design-system-options
```

Or install it for one project only:

```bash
git clone https://github.com/EngSharaqi/design-system-skill.git .claude/skills/design-system-options
```

Restart Claude Code (or start a new session). The skill is listed as `design-system-options`.

To update later: `git -C ~/.claude/skills/design-system-options pull`.

## Usage

Open Claude Code **in the project you want a design system for** and just ask:

```
make a design system for this project
```
```
propose design directions for the customer portal only
```
```
اعملي ديزاين سيستم للبروجيكت
```

Or call it explicitly, with an optional scope:

```
/design-system-options
/design-system-options admin dashboard
```

What happens:

1. **Scope.** If it's unclear which part of the product you mean, you get **one** question.
2. **Reading the project.** The skill reads your stack, stylesheets, tokens, fonts, logo and brand
   files, language and direction, and your most-used screen. If it can't find a brand, it asks once
   for colours or a guide. If you have none, it derives the palette from your current UI.
3. **Building and checking.** It builds `index.html`, screenshots every option on desktop and
   mobile, checks for horizontal scroll, and computes every contrast ratio.
4. **Choosing.** You get the link and a short summary, then a question: **A, B, C or a mix?** For a
   mix, you choose a source per layer. The open decisions are asked in the same round.
5. **The approved reference.** It writes `final.html`, `tokens.css` and `DESIGN.md`.

You can also approve later, in a new session:

```
/design-system-options approve b
/design-system-options approve mostly A, header and focus from B, keep Inter
```

### Where the files go

In order of priority:

1. the folder you name in your request;
2. a deliverables location your `CLAUDE.md` or rules define;
3. `./design-system/<YYYY-MM-DD>/` at the project root.

### After approval

The skill **offers** two next steps and does neither until you say so:

- wiring `tokens.css` into the codebase (CSS variables, Tailwind `@theme`, SCSS variables…);
- adding a line to the project's `CLAUDE.md`, e.g. *"UI work follows `design-system/2026-10-29/DESIGN.md`"*,
  so every later Claude session designs within the approved system.

## What it reads, and what it never does

**Reads:** package manifests, CSS/SCSS/LESS, Tailwind config, component files, i18n files, brand
assets in the repo and its parent folder. It counts the values actually in use (for example "11
font sizes between 11 and 34px; 47 hard-coded hex colours"). Those counts show up in the
comparison and in the do/don'ts.

**Never, unless you ask:** edits your source code, commits, pushes, publishes or uploads anything.
All output goes into the design-system folder.

## Built-in quality bar

- Text contrast ≥ 4.5:1 and UI/focus contrast ≥ 3:1. Every ratio on the page is computed, none
  is typed in from memory.
- Mobile-first scale. Controls grow on touch screens (32/40/48 → 40/48/56px). Body text stays 16px
  so iOS doesn't zoom into fields.
- Logical CSS properties throughout, so the same file works LTR and RTL. Directional icons mirror.
  Arabic, Hebrew, Persian and Urdu projects are handled as RTL-first.
- `prefers-reduced-motion` respected. Tabular numerals in tables.
- One file, no framework, no build. The only external requests go to Google Fonts.

## Requirements

- Claude Code.
- Google Chrome or Chromium for the screenshot checks. Without it, the skill says so and skips that
  step.
- Nothing to install in your project.

## Repository layout

```
SKILL.md                     # the workflow Claude follows
references/page-contract.md  # anatomy and rules of the options page
references/approval.md       # phase 2: resolving a mix, final.html, tokens.css, DESIGN.md
assets/template.html         # neutral starting page (demo palette, FILL markers)
```

`assets/template.html` uses an obviously fake demo palette and placeholder copy. The skill must
replace every demo value and `<!-- FILL -->` marker, and it checks that none survive, so nothing
from the template's look leaks into your project.

## Customising

- **Different default directions:** edit the table in `SKILL.md` § 4, for example to make B follow
  your company's design system.
- **Different component set:** edit `#components` in the template and the list in
  `references/page-contract.md`.
- **Different output folder:** state it in your `CLAUDE.md`. The skill honours it before its
  default.

---

## بالعربي

سكيل لـ Claude Code تقرأ أي مشروع (الكود والألوان والخطوط والهوية والشاشة الأكثر استخدامًا)، وتطلع
**صفحة واحدة فيها ثلاث خيارات لنظام التصميم** تبدّل بينها بضغطة: **أ** امتداد للموجود، **ب** مؤسسي
ومنضبط، **ج** الهوية فيه أوضح. الخيارات الثلاثة تتفق في الأساسات (الألوان والخطوط والمسافات
والمقاسات)، وتختلف في الشكل والعمق ومكان لون الهوية. وفي الصفحة جدول مقارنة، ومراجع لكل رقم،
والمكوّنات كلها بحالاتها، وشاشة من مشروعك نفسه.

بعد ما تختار خيارًا، أو خليطًا تحدّد فيه مصدر كل طبقة، تطلع لك **النسخة المعتمدة**:
- `final.html`: المرجع المرئي.
- `tokens.css`: المتغيّرات.
- `DESIGN.md`: القواعد اللي أي تصميم بعد كده يمشي عليها.

**التركيب:**
```bash
git clone https://github.com/EngSharaqi/design-system-skill.git ~/.claude/skills/design-system-options
```

**الاستخدام:** افتح Claude Code في المشروع واكتب «اعملي ديزاين سيستم للبروجيكت» أو
`/design-system-options`. وللاعتماد بعدين: `/design-system-options approve ب`.

السكيل ما بتعدّلش كود المشروع، ولا بتعمل commit أو push أو تنشر أي حاجة، غير لما تطلب.
