# HeyGent PM Skills

A Claude Code plugin with 19 workflow skills (+ interactive onboarding) covering the full loop from customer discovery → planning → PRD → tech plan → review → learn, plus competitor analysis, knowledge ingest, validation storyboards, Mixpanel data analysis, a brand-agnostic design-system toolkit (extract your DS from assets, build, compliance audit, prototype → Figma), and PRD → Jira Epic breakdown.

> Forked from [yahellif-ele/pm-design-agents](https://github.com/yahellif-ele/pm-design-agents) (MIT, © Ran Erez). The fork removes vendor-specific branding and turns the design-system skills into a configurable, bring-your-own-DS toolkit.

## Skills

| # | Slash command | What it does |
|---|---------------|--------------|
| 00 | `/00-onboarding` | Guided first-time setup: fill `CLAUDE.md` and `workspace-tools.md`, tour installed skills, get a personalized next step. |
| 01 | `/01-customer-discovery` | Synthesize customer feedback (meetings, calls, notes, CSVs) into the top user-problem trends. Read-only. |
| 02 | `/02-pm-planner` | Turn an unstructured problem space into 2–3 initiative candidates, then a focused kickoff one-pager. |
| 03 | `/03-cto-planner` | Stress-test an initiative against the existing codebase; surface gaps, edge cases, open questions. |
| 04 | `/04-ux-planner` | Produce a UX plan: users, journeys, IA, key screens/states, UX acceptance criteria. |
| 05 | `/05-create-prd` | Consolidate 01–04 outputs into a decision-ready feature PRD (GIFTS); saves to `Outputs/Product PRDs/`. |
| 06 | `/06-prd-to-tech-plan` | Write a concise, implementation-ready Technical Design Doc to `Outputs/Technical Docs/`. |
| 07 | `/07-ui-ux-review` | Review PRD + UI implementation; return prioritized feedback and concrete fixes. |
| 08 | `/08-rnd-reviewer` | Review PRD/TDD + implementation as a senior engineer. |
| 09 | `/09-pm-reviewer` | Review PRD + shipped experience as a PM (problem, scope, trade-offs, metrics, rollout). |
| 10 | `/10-learn` | Ship retro (update PRD/TDD) and/or process–skill improvement; writes to `Learnings/`. |
| 11 | `/11-competitor-feature-analysis` | Capture logged-in competitor product UI via CloakBrowser, compare a feature across competitors, generate a report + HTML deck. |
| 12 | `/12-ingest-knowledge` | Ingest external docs (Drive, Zoom, notetakers, Gmail, local Inbox) into `Knowledge/` as structured cards. Propose-then-confirm. |
| 13 | `/13-validation-storyboard` | Capture a URL or product/demo video into a validation storyboard — screenshots + a checklist to validate. |
| 14 | `/14-mixpanel-data-analysis` | Investigate why a metric moved in Mixpanel, then build a live dashboard + a saved analysis memo in `Outputs/Analytics/`. |
| 15 | `/15-ds-build` | Build a DS-compliant screen, implement a Figma design as code with your own design system, or sync Figma changes back into the prototype. |
| 16 | `/16-ds-compliance-check` | Audit code (JSX/HTML/CSS) against your design system — hardcoded values, wrong imports, missing tokens/components — with exact fixes. |
| 17 | `/17-prototype-to-figma` | Rebuild a prototype screen (code/screenshot) as a DS-compliant Figma frame via the figma-console MCP. |
| 18 | `/18-prd-to-epic` | Turn a PRD + prototype into a Jira Epic with an atomic User-Story breakdown; writes `epic-<slug>.md` to `Outputs/Product PRDs/`. |
| 19 | `/19-ds-extract` | Generate your design system (`Knowledge/Design/ds-config.md`, `tokens.md`, `components.md`) from screenshots, logos, a brand PDF, a live URL, a Figma file or existing CSS/theme code — the setup step for skills 15–17. |

> Upgrading from 0.x? Skills 05–10 were renumbered to 06–11 to make room for `/05-create-prd`. See [CHANGELOG.md](CHANGELOG.md).

## Install

**Quick start.** Open a terminal app on your computer (macOS: Terminal.app; Windows: Windows Terminal or PowerShell) — **not** a Claude chat — and run:

```bash
claude plugin marketplace add heygents/heygent-pm-skills
claude plugin install heygent-pm-skills@heygent
```

> ⚠️ These are shell commands. They run in your terminal, not in a Claude conversation. If you paste them into Claude chat, claude.ai, or the Claude Code chat panel, you'll just get a text reply explaining the command — nothing will install.
>
> Already inside Claude Code (CLI or IDE)? You can alternatively use `/plugin marketplace add …` and `/plugin install …` directly there. Plugins do not work in the Claude desktop chat app or claude.ai.

Then bootstrap your workspace and run `/00-onboarding` in Claude Code (recommended) — or copy the templates and edit them manually.

**Full step-by-step walkthrough** (recommended for first install): [INSTALL.md](INSTALL.md).

## What you need to provide

The skills are vendor-agnostic. To make them work end-to-end, configure your own:

- **MCP servers**: tracker (Monday/Jira/Linear/Notion), analytics (Mixpanel/Amplitude/PostHog), email/calendar (Gmail/Outlook), docs (Notion/Google Drive). Configure these in your Claude Code MCP settings, then list their server names in `Knowledge/workspace-tools.md`.
- **Workspace folders**: `Outputs/`, `Learnings/`, and `Knowledge/` (with numbered subfolders `01-Templates` … `06-Projects`) at the workspace root. Skills write artifacts there.
- **PM context**: `CLAUDE.md` at workspace root — terminology, writing rules, sub-agent roles. The template ships everything except your company-specific fields.

## Per-skill prereqs

| Skill | Needs an MCP server? | Notes |
|-------|----------------------|-------|
| 01 customer-discovery | Tracker / docs MCP **OR** local CSV/folder paths | Set under "Customer Meeting Transcripts" in workspace-tools |
| 02–04 planning | Optional — works with pasted content too | MCP makes it pull context automatically |
| 05 create-prd | Tracker MCP optional (to post a comment on a linked issue) | Reads upstream 01–04 outputs; writes `Outputs/Product PRDs/` |
| 06–09 review | Optional — works with pasted content too | MCP makes it pull context automatically |
| 10 learn | Tracker MCP optional | Reads PRD/TDD from `Outputs/`, writes to `Learnings/` |
| 11 competitor-analysis | None — uses CloakBrowser locally | Needs Node 18+, macOS/Windows |
| 12 ingest-knowledge | Source MCPs (Drive/Zoom/notetakers/Gmail) **OR** local `Knowledge/_inbox/` | Set under "Knowledge Sources" in workspace-tools |
| 13 validation-storyboard | None — drives a local browser | Needs Node 18+ |
| 14 mixpanel-data-analysis | Mixpanel MCP (query + dashboard tools) | Reads NSM from `CLAUDE.md`; writes `Outputs/Analytics/` |
| 15 ds-build | `figma-console` MCP for Figma-sync / implement-from-Figma modes (optional otherwise) | Needs `Knowledge/Design/ds-config.md` filled (run `/19-ds-extract`); verify loop wants a local dev server |
| 16 ds-compliance-check | None — static code audit | Needs `Knowledge/Design/ds-config.md` filled (run `/19-ds-extract`) |
| 17 prototype-to-figma | `figma-console` MCP **required** (writes to Figma) | Needs a Figma file key in `Knowledge/Design/ds-config.md` |
| 18 prd-to-epic | Jira/tracker MCP optional (only to create/transition issues — with explicit approval) | Reads a PRD + prototype; writes `epic-<slug>.md` to `Outputs/Product PRDs/` |
| 19 ds-extract | `figma-console` MCP optional (only if a Figma file is one of the sources) | Reads screenshots/PDF/CSS/URL; writes `Knowledge/Design/ds-config.md`, `tokens.md`, `components.md` |

## Design system: bring your own

Skills 15–17 never assume a brand or a component library. Everything project-specific lives in `Knowledge/Design/ds-config.md` (DS name, `component-library` vs `tokens-only` mode, component package, forbidden imports, token formats, Figma file key, grep patterns). `bin/bootstrap.sh` seeds it as a template; `/19-ds-extract` fills it from whatever you have — screenshots, logos, a brand PDF, a live URL, a Figma file, or existing CSS / Tailwind / theme code — and every generated value is tagged `exact`, `inferred` or `generated`.

## Updating

```
/plugin update heygent-pm-skills@heygent
```

## Layout

```
.
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── README.md
├── LICENSE
├── skills/
│   ├── 00-onboarding/SKILL.md
│   ├── 01-customer-discovery/SKILL.md
│   ├── 02-pm-planner/SKILL.md
│   ├── 03-cto-planner/SKILL.md
│   ├── 04-ux-planner/SKILL.md
│   ├── 05-create-prd/SKILL.md
│   ├── 06-prd-to-tech-plan/SKILL.md
│   ├── 07-ui-ux-review/SKILL.md
│   ├── 08-rnd-reviewer/SKILL.md
│   ├── 09-pm-reviewer/SKILL.md
│   ├── 10-learn/SKILL.md
│   ├── 11-competitor-feature-analysis/      # SKILL.md + scripts + INSTALL.md
│   ├── 12-ingest-knowledge/SKILL.md
│   ├── 13-validation-storyboard/            # SKILL.md + scripts
│   ├── 14-mixpanel-data-analysis/SKILL.md
│   ├── 15-ds-build/SKILL.md
│   ├── 16-ds-compliance-check/SKILL.md
│   ├── 17-prototype-to-figma/SKILL.md
│   ├── 18-prd-to-epic/SKILL.md
│   └── 19-ds-extract/SKILL.md
└── templates/
    ├── CLAUDE.md.template
    └── Knowledge/
        ├── workspace-tools.md.template
        ├── competitors.md.template
        ├── 01-Templates/README.md.template
        ├── 02-Product-Knowledge/README.md.template
        ├── 03-Market-Knowledge/README.md.template
        ├── 04-ICP/README.md.template
        ├── 05-Workspace-Tools/README.md.template
        ├── 06-Projects/README.md.template
        └── Design/            # design.md (universal rules) + ds-config.md, tokens.md, components.md templates
```

## License

MIT — see `LICENSE`. Original work © 2026 Ran Erez (pm-design-agents); modifications © 2026 HeyGent.
