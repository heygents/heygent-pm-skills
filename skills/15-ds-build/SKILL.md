---
name: 15-ds-build
description: Build a DS-compliant screen from scratch, implement a Figma design as code using the workspace's own design system (configured in `Knowledge/Design/ds-config.md`), or sync Figma changes back into the prototype codebase. Use when the user says "build this screen", "add this feature", "prototype this", "implement this design", "push to prototype", "sync Figma changes", or runs /15-ds-build.
---

# DS Build

> Read `Knowledge/Design/design.md` and `Knowledge/Design/ds-config.md` before starting.

## Preflight — load the DS config

`ds-config.md` is the only place brand and library specifics live. Read these values and use them everywhere below:

| Variable | From `ds-config.md` |
|---|---|
| `DS_NAME` | Project Identity → Design system name |
| `MODE` | Implementation mode → `component-library` / `tokens-only` |
| `PACKAGE` | Code → Component package (`none` in tokens-only mode) |
| `FORBIDDEN` | Code → Forbidden imports |
| `TOKEN_CSS` / `TOKEN_JS` | Code → Token formats |
| `SCAN_*` | Static scan patterns table |

If `ds-config.md` still contains `[FILL]` → **stop**:
> "Your design system isn't configured yet. Run `/19-ds-extract` with your brand assets (screenshots, CSS, Figma, brand PDF) or fill `Knowledge/Design/ds-config.md` by hand, then re-run."

---

## Step 0 — Detect mode

Infer from the prompt:

| Mode | Signal |
|---|---|
| **Implement** | Figma URL, screenshot, or detailed spec → translate to code |
| **Design then build** | Description only, no visual → plan layout first, confirm, then build |
| **Figma sync** | "push to prototype", "sync Figma changes", "update from Figma" → pull Figma diff, apply to existing code |

If ambiguous between Implement and Design-then-build, ask one question only:
> "Do you have a visual reference (Figma / screenshot), or should I design the layout from scratch?"

Figma sync is always unambiguous — proceed directly to Step 0a. If `ds-config.md` lists the Figma file key as `n/a`, Figma sync is unavailable: say so and offer Implement / Design-then-build instead.

---

## Step 0a — Figma sync: verify connection

Call `figma_get_status` first.
- Connected → proceed to Step 1b
- Not connected → **stop**:
  > "Figma Desktop Bridge is unavailable. Cannot pull design changes. Please reconnect and try again."

---

## Step 1a — Implement / Design: plan first

Before any code, declare:

```
### Build Plan — [name]

Mode: Implement / Design then build
Layout: [Stack / Grid / Box — which DS layout components]

Components:
| Element | DS Component | Variant | Size |
|---|---|---|---|
| [el] | [Name] | [variant=] | [size=] |

Token-only elements:
| Element | Token |
|---|---|
| [bg] | color/background/default |

Gaps:
⚠️ MISSING COMPONENT: [name] — [use case]

Files: [paths to create/edit]
```

Wait for confirmation before writing.

---

## Step 1b — Figma sync: diff first

Pull:
```
figma_get_design_context → target frame
figma_get_component_for_development_deep → instances + tokens
```
Source: Figma Console — [node ID] — fetched now

Read current prototype files.
Source: [filepath] — read from repo

Diff table:

| Element | Figma (new) | Prototype (current) | Action |
|---|---|---|---|
| Button variant | contained | outlined | Update |
| Gap | spacing.300 | 16px hardcoded | Update + fix token |
| Sidebar | removed | present | Confirm before removing |

Before applying — always confirm:
- Removals → explicit confirmation required
- Prototype-only elements (event handlers, state, analytics) → preserve, never overwrite
- Conflicts (both sides changed) → show both versions, ask user, never silently overwrite
- Backup: cp [file] [file].bak before writing

---

## Step 2 — Avoid slop (Implement / Design modes)

- No hardcoded hex, rgb(), arbitrary px
- No style={{}} inline styles
- No import from any package in `FORBIDDEN` — always from `PACKAGE`
- `tokens-only` mode: no component library at all; compose from tokens (`TOKEN_CSS`) and plain markup, and document each reusable piece
- No recreating a DS component that already exists
- Every visual value is a token reference or DS component prop

---

## Step 3 — Build / Apply

Implement / Design: write code per plan.
Figma sync: apply diff one screen at a time, confirm after each.

---

## Step 4 — Verify loop (run all three)

1. Token grep:
   grep -rn 'SCAN_COLORS\|SCAN_INLINE\|SCAN_FORBIDDEN' [filepath]   # substitute the patterns from ds-config.md; drop SCAN_FORBIDDEN in tokens-only mode
   Should return zero results.

2. Cross-reference:
   Check `Knowledge/Design/components.md` — confirm every component used exists in `DS_NAME`
   with the exact variant and size used.

3. Render and compare:
   Run dev server → screenshot → compare to input.
   List discrepancies before marking done.

---

## Step 5 — Self-review

1. Every visual value is a DS prop or token — zero exceptions?
2. Verify loop passed all three checks?
3. Any DS gaps silently worked around instead of flagged?

---

## Step 6 — Report

```
## DS Build: Done

### Files
- [path]: [created / modified]

### DS usage
- [N] `PACKAGE` components (or token-composed pieces in tokens-only mode)
- [N] token-only primitives

### Gaps flagged
- MISSING COMPONENT: [name] — annotated as TODO

### Verify
- Token grep: 0 hardcoded values
- Cross-reference: confirmed in Knowledge/Design/components.md
- Render: matches input / [discrepancy if any]
```
