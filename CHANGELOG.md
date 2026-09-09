# Changelog

All notable changes to this plugin are documented here. Format follows [Keep a Changelog](https://keepachangelog.com/) loosely.

This project is a fork of [yahellif-ele/pm-design-agents](https://github.com/yahellif-ele/pm-design-agents) at its v1.9.0 (2026-06-28). Its earlier history lives in that repo's `CHANGELOG.md`.

## Unreleased

## 1.0.1 — 2026-09-09

### Changed
- **Marketplace moved.** The `heygent` marketplace manifest now lives in its own repo, [heygents/heygent-skills](https://github.com/heygents/heygent-skills), so it can list every HeyGent plugin (this one and `ayal-doron-methodology-skills`). `.claude-plugin/marketplace.json` was removed from this repo. Fresh installs: `claude plugin marketplace add heygents/heygent-skills`. Existing users: `claude plugin marketplace remove heygent && claude plugin marketplace add heygents/heygent-skills && claude plugin install heygent-pm-skills@heygent` (removing a marketplace also removes its plugins; workspace files are untouched).

### Fixed
- YAML frontmatter of `/01-customer-discovery`, `/04-ux-planner` and `/19-ds-extract` failed to parse (unquoted `description` with special characters), so their `name`/`description` were silently dropped at load time. Descriptions are now quoted; `claude plugin validate` passes.

## 1.0.0 — 2026-09-06

First HeyGent release. Functionally equivalent to upstream v1.9.0 except where noted.

### Changed
- **Rebranded** as `heygent-pm-skills` in the `heygent` marketplace (`heygents/heygent-pm-skills`). Install path is now `~/.claude/plugins/cache/heygent/heygent-pm-skills/<version>/`. All vendor branding and personal contact details removed; README, INSTALL, onboarding and scripts updated.
- **Design-system skills are now brand-agnostic.** `/15-ds-build`, `/16-ds-compliance-check` and `/17-prototype-to-figma` no longer assume a specific component library or Figma file. Each starts with a preflight that loads `Knowledge/Design/ds-config.md` (DS name, `component-library` / `tokens-only` mode, component package, forbidden imports, token formats, Figma key, grep patterns) and stops with a pointer to `/19-ds-extract` if the file still has `[FILL]` placeholders. New `tokens-only` mode supports products without a component package.
- **`templates/Knowledge/Design/`** — `ds-config.md` and `tokens.md` are now generic templates with `[FILL]` placeholders and a per-value confidence column; `components.md` and `design.md` lost their vendor references. `design.md` (universal rules) is unchanged otherwise.
- `bin/validate-workspace.sh` warns when `ds-config.md` is still a template.

### Added
- **New skill `/19-ds-extract`.** Generates `ds-config.md`, `tokens.md` and `components.md` from graphic and code sources — screenshots, logos, brand-guideline PDFs, a live URL, a Figma file, or existing CSS / Tailwind / theme code. Detects implementation mode, assigns semantic color roles, builds primitive scales, extracts typography/spacing/radius/elevation, catalogs visible components, runs WCAG AA contrast checks, and tags every value `exact` / `inferred` / `generated`. Propose-then-confirm; optional `Outputs/Design/tokens.css` + `tokens.json` export.

### Upgrade path
Fresh install:
1. `claude plugin marketplace add heygents/heygent-pm-skills`
2. `claude plugin install heygent-pm-skills@heygent`
3. Restart Claude Code, then `bash ~/.claude/plugins/cache/heygent/heygent-pm-skills/1.0.0/bin/bootstrap.sh` in your workspace.
