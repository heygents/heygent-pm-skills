---
name: 16-ds-compliance-check
description: Audits code (JSX/HTML/CSS) or a file path for compliance with the workspace's design system (configured in `Knowledge/Design/ds-config.md`) — hardcoded values, wrong imports, missing tokens/components — and reports violations with exact fixes. Use when the user says "audit this", "check DS compliance", "find violations", "is this using the DS?", or runs /16-ds-compliance-check.
---

# DS Compliance Check — Code

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

## Input accepted

- File path(s) in the repo
- Code snippet (JSX / HTML / CSS / SCSS)
- Component name ("audit the Header component")

---

## Step 1 — Gather source

If file path provided:
```bash
# Read the file(s)
cat [filepath]
```

State: `📡 Source: [filepath] — read from repo`

---

## Step 2 — Run compliance checks

Check each category. Mark: ✅ Pass / ⚠️ Violation / ❓ Can't determine statically

### Imports (skip in `tokens-only` mode)
- [ ] All UI components imported from `PACKAGE` — never from anything in `FORBIDDEN`
- [ ] No DS components recreated locally that exist in the library

### Tokens — Colors
- [ ] Zero hardcoded hex values (`#...`)
- [ ] Zero hardcoded `rgb()` / `rgba()` color values
- [ ] Colors via DS component props (`color="primary"`), theme path (`TOKEN_JS`) or CSS variable (`TOKEN_CSS`)

### Tokens — Spacing
- [ ] Zero arbitrary `px` values in padding/margin/gap
- [ ] Spacing via DS `spacing.*` tokens or the *Spacing helper* from `ds-config.md`

### Tokens — Typography
- [ ] Zero inline font-size / font-weight / font-family
- [ ] Typography via the *Typography component* from `ds-config.md` (or text-style tokens in tokens-only mode)

### Tokens — Other
- [ ] Zero hardcoded border-radius values — use `border.radius.*`
- [ ] Zero hardcoded box-shadow values — use elevation tokens / prop

### Component usage
- [ ] Correct variant for context (no `contained` where `text` is appropriate)
- [ ] Correct size prop set explicitly
- [ ] No detached/custom replacements for DS components

---

## Step 3 — Static scan (run these)

Take each pattern from the *Static scan patterns* table in `ds-config.md`:

```bash
# Hardcoded colors
grep -rn 'SCAN_COLORS' [filepath]

# Wrong import source (skip when SCAN_FORBIDDEN is empty / tokens-only mode)
grep -rn 'SCAN_FORBIDDEN' [filepath]

# Inline styles
grep -rn 'style={{' [filepath]

# Arbitrary px in sx prop (rough signal)
grep -rn '"[0-9]\+px"' [filepath]
```

---

## Step 4 — Report

```markdown
## DS Compliance Audit — [File / Component]
📡 Source: [filepath] — read from repo
Date: [today]

### Summary
✅ Passing: [N]
⚠️ Violations: [N]
❓ Can't determine: [N]

### Violations

| # | Location | Violation | Rule | Fix |
|---|---|---|---|---|
| 1 | [file:line] | `style={{ color: '#666' }}` | No hardcoded colors | Use `color/text/secondary` via `TOKEN_JS` / `TOKEN_CSS` |

### Scan results
- Hardcoded colors: [N] instances
- Wrong imports: [N] instances
- Inline styles: [N] instances

### Verdict
[ ] Clean — ready for review
[ ] Needs fixes — see violations above
```

---

## Self-review

1. Did I run all four grep checks, not just visual scan?
2. For every violation — did I provide the exact fix (token name / component prop)?
3. Did I distinguish confirmed violations from "can't determine statically"?
