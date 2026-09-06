---
name: 17-prototype-to-figma
description: Reads a prototype screen (code, screenshot, or description) and rebuilds it as a DS-compliant Figma frame via the figma-console MCP. Use when the user says "bring this to Figma", "rebuild in Figma", "convert to Figma", "take this screen to Figma", or runs /17-prototype-to-figma.
---

# Prototype → Figma (Code)

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

If the Figma *File key* in `ds-config.md` is `n/a` → **stop**: this skill needs a Figma library to instantiate from. Add the key (or run `/19-ds-extract` with your Figma URL).

> This skill writes to Figma — figma-console MCP is required.

---

## Step 0 — Verify Figma connection

Call `figma_get_status` first.

- ✅ Connected → proceed
- ❌ Not connected → **stop**:
  > "Figma Desktop Bridge is unavailable. I can map which DS components and tokens
  > to use, but cannot build in Figma until the connection is restored.
  > Want me to prepare the component map instead?"

---

## Step 1 — Read the prototype

If file path provided → read the component file(s).
If screenshot → analyze visually.
State: `📡 Source: [filepath / screenshot] — [read now]`

---

## Step 2 — Map code → DS components

```markdown
### Component Map

| Code element | DS Component | Variant | Size | Notes |
|---|---|---|---|---|
| `<Button variant="contained">` | Button | contained | medium | — |
| `<div className="card">` | Card | — | — | ⚠️ custom — map to Card component |

### Gaps
| Code element | Flag |
|---|---|
| [element] | ⚠️ MISSING COMPONENT: [name] — [use case] |
```

---

## Step 3 — Build in Figma

Order: most complex component first.

1. Create frame with auto-layout on target page
2. For each DS component:
   - `figma_instantiate_component` — use component set ID from `Knowledge/Design/ds-config.md`
   - `figma_set_instance_properties` — variant/size/state
   - `figma_set_text` — text content from code
   - `figma_set_fills` — DS variable references only, no raw hex
3. `figma_capture_screenshot` after each major section → compare to prototype
4. Fix discrepancies before continuing

**Token rule:** Every fill, stroke, spacing, radius → DS variable. Zero raw values.

---

## Step 4 — Quality gates

- [ ] Every fill/stroke → DS variable
- [ ] Every spacing → DS token
- [ ] Every component → DS instance
- [ ] Screenshot matches prototype
- [ ] Custom/missing elements annotated: `[NOT IN DS — ⚠️ MISSING COMPONENT]`

---

## Step 5 — Report

```
## Prototype → Figma: Done
📡 Figma: [node link]

### Built
- [Screen]: [link]

### Mapping
- [N] DS components instantiated
- [N] token-only primitives

### Gaps
- ⚠️ MISSING COMPONENT: [name] — annotated in Figma

### Discrepancies from prototype
- [any layout / visual differences noted]
```

---

## Self-review

1. Every element is either a DS instance or a token-bound primitive — zero exceptions?
2. Did I screenshot and compare after each section?
3. Every gap flagged — nothing silently worked around?
