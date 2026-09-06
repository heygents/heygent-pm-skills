---
name: tokens
description: >
  Brand-specific design tokens for [FILL: design system name].
  Load AFTER design.md. Brand-specific — do not reuse for another DS.
  Fill every [FILL] by hand, or generate this file with /19-ds-extract.
  Last verified: [FILL: YYYY-MM-DD]
---

# [FILL: DS name] — Design Tokens ([FILL: theme, e.g. Default / Light])

> ⚠️ Token values verified [FILL: YYYY-MM-DD]. Re-fetch from live sources before using IDs in scripts.
> Naming convention: Figma `color/x/y` → JS `[FILL: e.g. theme.palette.x.y]` → CSS `[FILL: e.g. var(--acme-color-x-y)]`
> Confidence legend: **exact** = read from code/Figma · **inferred** = sampled from screenshots · **generated** = derived scale from a seed value

---

## Colors — Semantic

Every role below must exist. If the brand has no value for a role, derive one and mark it `generated`.

| Token | CSS variable | Value | Confidence |
|---|---|---|---|
| `color/primary/main` | `[FILL]` | `[FILL #hex]` | [FILL] |
| `color/primary/light` | `[FILL]` | `[FILL]` | [FILL] |
| `color/primary/dark` | `[FILL]` | `[FILL]` | [FILL] |
| `color/primary/contrastText` | `[FILL]` | `[FILL]` | [FILL] |
| `color/secondary/main` | `[FILL]` | `[FILL]` | [FILL] |
| `color/secondary/light` | `[FILL]` | `[FILL]` | [FILL] |
| `color/secondary/dark` | `[FILL]` | `[FILL]` | [FILL] |
| `color/secondary/contrastText` | `[FILL]` | `[FILL]` | [FILL] |
| `color/error/main` | `[FILL]` | `[FILL]` | [FILL] |
| `color/error/contrastText` | `[FILL]` | `[FILL]` | [FILL] |
| `color/warning/main` | `[FILL]` | `[FILL]` | [FILL] |
| `color/warning/contrastText` | `[FILL]` | `[FILL]` | [FILL] |
| `color/info/main` | `[FILL]` | `[FILL]` | [FILL] |
| `color/info/contrastText` | `[FILL]` | `[FILL]` | [FILL] |
| `color/success/main` | `[FILL]` | `[FILL]` | [FILL] |
| `color/success/contrastText` | `[FILL]` | `[FILL]` | [FILL] |
| `color/text/primary` | `[FILL]` | `[FILL]` | [FILL] |
| `color/text/secondary` | `[FILL]` | `[FILL]` | [FILL] |
| `color/text/disabled` | `[FILL]` | `[FILL]` | [FILL] |
| `color/background/default` | `[FILL]` | `[FILL]` | [FILL] |
| `color/background/paper` | `[FILL]` | `[FILL]` | [FILL] |
| `color/divider` | `[FILL]` | `[FILL]` | [FILL] |
| `color/action/hover` | `[FILL]` | `[FILL]` | [FILL] |
| `color/action/disabled` | `[FILL]` | `[FILL]` | [FILL] |

### State tokens (opacity overlays)

| Token | Opacity |
|---|---|
| `color/primary/states/hover` | [FILL — e.g. 4% of primary.main] |
| `color/primary/states/selected` | [FILL — e.g. 8%] |
| `color/primary/states/focus` | [FILL — e.g. 12%] |
| _(same pattern for secondary, error, warning, info, success)_ | |

### Contrast check (WCAG 2.1 AA)

| Pair | Ratio | Pass? |
|---|---|---|
| text/primary on background/default | [FILL] | [FILL ≥ 4.5:1] |
| primary/contrastText on primary/main | [FILL] | [FILL ≥ 4.5:1] |
| text/secondary on background/paper | [FILL] | [FILL ≥ 4.5:1] |

---

## Colors — Primitive Scale

One row per hue the brand actually uses. A `generated` scale is fine as a starting point — mark it and confirm with design.

| Scale | 50 | 100 | 200 | 300 | 400 | 500 | 600 | 700 | 800 | 900 | Confidence |
|---|---|---|---|---|---|---|---|---|---|---|---|
| **grey** | [FILL] | | | | | | | | | | |
| **[FILL brand hue]** | [FILL] | | | | | | | | | | |
| **red** | [FILL] | | | | | | | | | | |
| **yellow** | [FILL] | | | | | | | | | | |
| **blue** | [FILL] | | | | | | | | | | |
| **green** | [FILL] | | | | | | | | | | |

---

## Typography

### Font Family

| Role | Family | Fallback stack | Source |
|---|---|---|---|
| Headings | [FILL] | [FILL] | [FILL — @font-face / brand PDF / inferred] |
| Body | [FILL] | [FILL] | [FILL] |
| Mono / data | [FILL or n/a] | [FILL] | [FILL] |

### Font Size Scale

| Token | Value | Token | Value |
|---|---|---|---|
| `font.size.100` | [FILL — e.g. 12px] | `font.size.400` | [FILL — e.g. 24px] |
| `font.size.150` | [FILL — 14px] | `font.size.500` | [FILL — 32px] |
| `font.size.200` | [FILL — 16px] | `font.size.600` | [FILL — 40px] |
| `font.size.300` | [FILL — 20px] | `font.size.700` | [FILL — 48px] |

### Other Typography Tokens

| Token | heading | subtitle | body | button |
|---|---|---|---|---|
| `font.weight.*` | [FILL] | [FILL] | [FILL] | [FILL] |
| `letterSpacing.*` | [FILL] | [FILL] | [FILL] | [FILL] |
| `lineHeight.*` | [FILL] | [FILL] | [FILL] | [FILL] |

### Typography Variants

[FILL — e.g. `h1–h6` `subtitle1` `subtitle2` `body1` `body2` `caption` `overline`, and how each maps to an HTML element]

---

## Spacing

Base unit: [FILL — e.g. 4px / 8px]

| Token | Value | Token | Value |
|---|---|---|---|
| `spacing.0` | 0 | `spacing.400` | [FILL] |
| `spacing.50` | [FILL] | `spacing.500` | [FILL] |
| `spacing.100` | [FILL] | `spacing.600` | [FILL] |
| `spacing.150` | [FILL] | `spacing.700` | [FILL] |
| `spacing.200` | [FILL] | `spacing.800` | [FILL] |
| `spacing.300` | [FILL] | | |

---

## Border

### Radius

| Token | Value |
|---|---|
| `border.radius.sm` | [FILL] |
| `border.radius.md` | [FILL] |
| `border.radius.lg` | [FILL] |
| `border.radius.circle` | 50% |

### Size

| Token | Value |
|---|---|
| `border.size.sm` | [FILL — e.g. 1px] |
| `border.size.md` | [FILL — e.g. 2px] |

---

## Elevation / Shadows

| Token | Value | Use |
|---|---|---|
| `elevation.0` | none | flat surfaces |
| `elevation.1` | [FILL — e.g. `0 1px 2px rgba(0,0,0,.08)`] | cards at rest |
| `elevation.2` | [FILL] | raised / hover |
| `elevation.3` | [FILL] | popovers, dialogs |

---

## Motion (optional)

| Token | Value |
|---|---|
| `motion.duration.fast` | [FILL — e.g. 120ms] |
| `motion.duration.base` | [FILL — e.g. 200ms] |
| `motion.easing.standard` | [FILL — e.g. `cubic-bezier(.2,0,0,1)`] |

---

## Breakpoints (optional)

| Token | Value |
|---|---|
| `breakpoint.sm` | [FILL] |
| `breakpoint.md` | [FILL] |
| `breakpoint.lg` | [FILL] |
