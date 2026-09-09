---
name: 19-ds-extract
description: "Generate a design system for this workspace — `Knowledge/Design/ds-config.md`, `tokens.md` and `components.md` — from graphic and code sources: screenshots, logos, a brand-guideline PDF, a live URL, a Figma file, or existing CSS / Tailwind / theme code. This is what makes `/15-ds-build`, `/16-ds-compliance-check` and `/17-prototype-to-figma` work against YOUR brand. Use when the user says \"create a design system from this\", \"extract tokens\", \"set up my DS\", \"here's my brand\", \"make the DS skills work for us\", or runs /19-ds-extract."
---

# DS Extract — Design system from your assets

> Read `Knowledge/Design/design.md` first (the brand-agnostic rules). This skill **writes** the three brand-specific files that sit next to it.
> Output: `Knowledge/Design/ds-config.md`, `Knowledge/Design/tokens.md`, `Knowledge/Design/components.md` — optionally `Outputs/Design/tokens.css` and `Outputs/Design/tokens.json`.

**Transparency rule (from design.md §1) applies to every value you produce:** each token carries a confidence — `exact` (read from code / Figma), `inferred` (sampled from an image or a live page), or `generated` (a scale derived from a seed value). Never present an inferred or generated value as exact.

---

## Step 0 — Preflight

1. Confirm `Knowledge/Design/` exists. If not: "Run the plugin's `bin/bootstrap.sh` from your workspace first."
2. Read `Knowledge/Design/ds-config.md`.
   - Still contains `[FILL]` → fresh extraction, continue.
   - Already filled → ask one question: **"Update the existing DS in place, or replace it?"** Either way, back up first: `cp ds-config.md ds-config.md.bak` (same for `tokens.md`, `components.md`).

---

## Step 1 — Collect sources

Ask for whatever the user has. Any single source is enough to start; more sources raise confidence.

| Source type | What to ask for | How to read it | Yields |
|---|---|---|---|
| **Code** | paths to `tokens.css` / `theme.ts` / `tailwind.config.*` / `_variables.scss` / `package.json` | `cat` / Read | exact colors, type, spacing, radius; component package name |
| **Component library** | the package name or repo path | `ls` components dir, read exports / Storybook | exact component list, props, variants |
| **Figma file** | file URL | `figma_get_status` → variables & styles via `figma-console` MCP | exact tokens + component sets and their IDs |
| **Screenshots / mockups** | image paths (PNG/JPG) | Read (visual) | inferred colors, type, spacing, visible components |
| **Logo / brand marks** | SVG/PNG paths | Read; for SVG grep `fill=` / `stroke=` | exact brand hues (SVG) or inferred (raster) |
| **Brand guideline PDF** | path | Read | exact hex/Pantone, font names, usage rules |
| **Live URL** | one or more page URLs | in-app browser if available; otherwise skill 13's `capture-url-auto.mjs` to screenshot, then read computed CSS: `getComputedStyle` on body, headings, buttons, links | inferred → exact for CSS variables found in the page |

For each source, declare it:

```
📡 Source: [type] — [path / URL / node] — fetched now
```

Stop and ask if **no** source is available. Do not invent a brand.

---

## Step 2 — Detect implementation mode

| Evidence | Mode |
|---|---|
| `package.json` depends on an internal UI package (`@org/ui`, `@org/design-system`, …), or a components dir with exported React/Vue/Web components exists | `component-library` |
| Only CSS/Tailwind/tokens, or only visual sources | `tokens-only` |

Also record **forbidden imports**: the base library the DS wraps (`@mui/material`, `antd`, `@chakra-ui/react`, `@radix-ui/*` …) if the component package re-exports it. `none` otherwise.

Confirm with the user in one line before continuing:
> "I'll set this up as **[mode]** with component package **[name / none]**. Correct?"

---

## Step 3 — Extract

Work through every section of `tokens.md`. Prefer code > Figma > PDF > live page > screenshot when sources disagree, and note the disagreement.

### 3.1 Colors
- Collect every distinct color. From code/Figma/SVG/PDF: exact. From images: sample the dominant and accent colors, cluster near-duplicates (ΔE < 5), snap to hex → `inferred`.
- Assign **semantic roles**: primary, secondary, error, warning, info, success, text (primary/secondary/disabled), background (default/paper), divider, action (hover/disabled). A role with no candidate gets a derived value → `generated` (e.g. error from a standard red, hover as 4–8 % primary overlay).
- Build a **primitive scale 50–900** for grey and each brand hue. If only one seed exists, generate the ramp by lightness steps and mark the whole row `generated`.
- Run **WCAG AA contrast** on the three pairs in the template. Flag every failure: `⚠️ CONTRAST: [pair] = [ratio] — below 4.5:1 — [suggested darker/lighter value]`.

### 3.2 Typography
- Families: from `@font-face` / `font-family` / PDF (exact) or visual identification (`inferred` — say which faces it resembles and ask the user to confirm).
- Size scale: collect observed sizes, round to the nearest step of a 4 px-based scale, fill the `font.size.*` table. Weights, line-heights, letter-spacing per role.

### 3.3 Spacing
- Measure gaps/paddings observed (code exact; screenshot approximate). Infer the base unit (4 or 8 px). Emit the `spacing.*` scale. Mark `generated` for steps not observed.

### 3.4 Radius, borders, elevation, motion, breakpoints
- Same approach. Shadows from code are exact; from screenshots use three generic elevation levels marked `generated`.

### 3.5 Components
- **Component-library mode:** list every exported component, its props/variants/sizes from source or Storybook → `exact`. Follow the documentation standard in `components.md`.
- **Tokens-only mode / visual sources:** list every recurring UI pattern visible (buttons and their variants, inputs, cards, nav, tabs, modals, toasts, tables…). Document observed variants and states → `inferred`. Mark everything else `⚠️ NOT OBSERVED`.
- Fill the Quick Index and the **Gap Log** with anything the brand clearly needs but you could not find.

### 3.6 Figma
- If a Figma URL was given and `figma_get_status` is connected: record file key, variable collections and modes, and component-set IDs (with `verified: YYYY-MM-DD`).
- Otherwise write `n/a`.

---

## Step 4 — Propose (wait for confirmation)

Show a compact summary before writing anything:

```
### DS Proposal — [DS name]

Mode: component-library / tokens-only · Package: [name / none] · Forbidden imports: [list / none]
Sources: [n] — [list]

| Section | Values | exact | inferred | generated | Flags |
|---|---|---|---|---|---|
| Semantic colors | 24 | 10 | 8 | 6 | 1 contrast ⚠️ |
| Primitive scales | 6 rows | … | … | … | |
| Typography | … | | | | families need confirmation |
| Spacing | … | | | | base unit 8px |
| Radius / border / elevation | … | | | | |
| Components | [n] | | | | [n] gaps |

Open questions (answer or say "go with your defaults"):
1. [e.g. "Is the accent orange a brand color or a one-off marketing color?"]
2. [e.g. "Heading face looks like Inter Display — confirm?"]
```

Wait. Apply the answers.

---

## Step 5 — Write

1. `Knowledge/Design/ds-config.md` — fill **every** `[FILL]`, including the *Static scan patterns* row for forbidden imports (an alternation like `@mui/material\|antd`, or empty) and the *Provenance* block (sources, date, overall confidence).
2. `Knowledge/Design/tokens.md` — full tables with the Confidence column filled.
3. `Knowledge/Design/components.md` — one entry per component following the file's documentation standard; code examples use the real import rule from `ds-config.md` (or plain HTML + CSS variables in tokens-only mode).
4. Do **not** edit `design.md`.
5. Optional, ask first: emit `Outputs/Design/tokens.css` (all tokens as CSS custom properties using the configured prefix) and `Outputs/Design/tokens.json` (W3C Design Tokens format). In tokens-only mode this is the recommended way to consume the DS in code.

---

## Step 6 — Verify

```bash
grep -n '\[FILL' Knowledge/Design/ds-config.md Knowledge/Design/tokens.md   # must be empty
```

- Every semantic color role has a value.
- Every value has a confidence.
- Contrast flags are listed, not hidden.
- If `Outputs/Design/tokens.css` was written: every token in `tokens.md` appears in it once.

---

## Step 7 — Report

```
## DS Extract: Done — [DS name]
📡 Sources: [list]

### Written
- Knowledge/Design/ds-config.md — mode: [..], package: [..]
- Knowledge/Design/tokens.md — [n] tokens ([exact]/[inferred]/[generated])
- Knowledge/Design/components.md — [n] components, [n] gaps logged
- Outputs/Design/tokens.css / tokens.json — [written / skipped]

### Flags to resolve
- ⚠️ CONTRAST: …
- ⚠️ CONFIRM FONT: …
- ⚠️ NOT OBSERVED: …

### Next
- `/16-ds-compliance-check <path>` — audit existing code against the new DS
- `/15-ds-build` — build a first screen with it
- `/17-prototype-to-figma` — only if a Figma file key is configured
```

---

## Self-review

1. Did every token get a confidence, and is nothing `inferred`/`generated` presented as exact?
2. Did I confirm mode + package with the user before writing?
3. Zero `[FILL]` left in `ds-config.md` and `tokens.md`?
4. Did I run the contrast check and surface every failure?
5. Did I leave `design.md` untouched?
