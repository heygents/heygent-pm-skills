---
name: components
description: >
  Brand-specific component reference. Load AFTER design.md.
  Replace this file with the actual component library for your brand/DS.
  This file is a TEMPLATE — fill in the sections below for your specific design system,
  or generate it with /19-ds-extract.
---

# Components Reference — [Brand / DS Name]

> **Load order:** design.md → tokens.md → **components.md** (this file)
> This file is brand-specific. Do not use values from this template without replacing them.

---

## How to Use This File

- Every component listed here is **available in the DS** — use it, do not recreate it
- If a component you need is **not listed** → flag it: `⚠️ MISSING COMPONENT`
- All prop values reference tokens from `tokens.md` — never raw values
- "Default" variant is marked with `*`

---

## Component Documentation Standard

Every component in this file follows this structure.
When adding a new component, use this template exactly:

```markdown
### [ComponentName]
> [One-line description of what it is and when to use it]

#### Variants & Props

| Prop | Values | Default |
|---|---|---|
| `variant` | `contained` \| `outlined` \| `text` | `text` |
| `size` | `small` \| `medium` \| `large` | `medium` |
| `color` | `primary` \| `secondary` \| `error` | `primary` |
| `disabled` | `boolean` | `false` |

#### States
| State | Visual | Notes |
|---|---|---|
| Default | [description] | — |
| Hover | [description] | — |
| Focused | [description] | Keyboard nav |
| Disabled | [description] | Not interactive |
| Loading | [description] | If applicable |

#### Do's and Don'ts
| ✅ Do | ❌ Don't |
|---|---|
| [Best practice] | [Anti-pattern] |
| Use for primary actions | Use more than one per view |

#### Accessibility
- **Role**: `button`
- **Keyboard**: Tab to focus, Enter/Space to activate
- **Screen reader**: Announced as button label
- **Disabled**: `aria-disabled="true"`, still focusable

#### Tokens Used
- Fill: `color/primary/main`
- Text: `color/primary/contrastText`
- Spacing: `spacing.200` (padding H), `spacing.100` (padding V)
- Radius: `border/medium`

#### Code Example
```jsx
// Import rule from ds-config.md (component-library mode); plain markup + CSS variables in tokens-only mode
import { Button } from '<component package>';

<Button variant="contained" color="primary" size="medium">
  Save changes
</Button>
```
```

---

## Quick Index

> Replace this with your actual component categories and names.

| Category | Components |
|---|---|
| Buttons | — |
| Inputs | — |
| Feedback | — |
| Navigation | — |
| Layout | — |
| Data Display | — |

---

## [Category 1 — e.g. Buttons]

> Add components here following the standard above.

---

## Patterns & Compositions

Common multi-component patterns used in this DS.
Document these when a combination of components is used repeatedly in the same way.

| Pattern | Components | When to use |
|---|---|---|
| [Pattern name] | [Component A + Component B] | [Context] |

---

## Deprecated Components

Components that exist in the DS but should no longer be used in new work.

| Component | Replaced by | Migration notes |
|---|---|---|
| [Old component] | [New component] | [How to migrate] |

---

## Gap Log

Running list of components that were needed but don't exist yet in the DS.
Every entry here should also be flagged with `⚠️ MISSING COMPONENT` in the relevant design file.

| Date | Missing component | Use case | Requested by | Status |
|---|---|---|---|---|
| [YYYY-MM-DD] | [component name] | [where needed] | [name] | Pending / In DS backlog / Shipped |
