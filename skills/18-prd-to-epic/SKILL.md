---
name: 18-prd-to-epic
description: Derives Jira Epic proposals from a Product PRD plus its prototype/mockup, broken into atomic, value-driven User Stories with acceptance criteria so the dev team can split each Epic into technical tasks. Writes an epic-<slug>.md per Epic under Outputs/Product PRDs/ and never creates or transitions Jira issues without explicit approval. Use when the user wants to turn a PRD + prototype into an Epic-level backlog, or runs /18-prd-to-epic.
---

# PRD + Prototype → Jira Epic

Turn the provided **PRD** and **prototype/mockup** into a **Jira Epic-level** backlog in Jira. Each epic is documented in a **markdown file** under **`Outputs/Product PRDs/`** and mirrored into the chat summary.

## Non-negotiables

1. **Context is King:** Use the PRD for business logic, scope, metrics, and edge cases. Use the prototype/`app/` routes to understand the UX and step-by-step user flow.
2. **Include User Stories:** Break the Epic down into atomic, value-driven User Stories. Do not write technical tasks or sub-tasks (leave DB schema and API design to the devs).
3. **Jira create gate:** **Never** create or transition Jira issues via MCP/API **without** explicit user approval. 
4. **Artifact on disk:** For **each** epic, write a markdown file under **`Outputs/Product PRDs/`** using the template below. Filename: `epic-<kebab-case-short-title>.md`.

## Epic Document Template (Mandatory)

```markdown
# Epic: [Short title]

> Jira: TBD | Prefix: [as appropriate for your project]

## User Problem & Value
[1-2 sentences from the PRD explaining what we are solving and why it matters to the user.]

## Success Definition (KPIs)
[How we measure success—e.g., >90% deploy success, time-to-live < 10 mins.]

## Scope Guarding
**In Scope for MVP:**
- [Strictly enforced feature boundaries]

**Out of Scope (Deferred):**
- [Explicitly state what we are NOT building yet to mitigate risk]

## High-Level User Flow
[Step-by-step happy path based on the prototype's UX.]

## User Story Breakdown
[Provide a list of User Stories that make up this Epic. The dev team will use these to create their technical tasks.]
- **Story 1:** As a [user persona], I want to [action] so that [value/outcome].
  - *Acceptance Criteria:* [High-level condition for done]
- **Story 2:** As a [user persona], I want to [action] so that [value/outcome].
  - *Acceptance Criteria:* [High-level condition for done]

## UX Design
[Figma: <full URL> | or TBD]

## Non-Functional Requirements (NFRs)
- [Performance, fallbacks, or security constraints directly from the PRD]
```
