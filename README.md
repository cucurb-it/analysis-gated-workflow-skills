# Analysis-Gated Workflow — Skill Set

A structured, phase-gated workflow for software development and refactoring,
governed by a Software Architect and supported by AI (Claude Code CLI or Copilot CLI).

---

## Skills

| Folder | Skill | Phase |
|---|---|---|
| `00-governing-workflow/` | Governing Workflow | All phases — orchestrator |
| `01-deep-feature-analysis/` | Deep Feature Analysis | Phase 01 |
| `02-deep-code-analysis/` | Deep Code Analysis | Phase 02 |
| `03-compliance-review/` | Compliance & Review | Phase 03 (iterative) |
| `04-implementation-planning/` | Implementation Planning | Phase 04 |
| `05-implementation/` | Implementation | Phase 05 & 06 |

---

## Installation

### Via Marketplace (recommended)

Once published to GitHub at `cucurb-it/analysis-gated-workflow-skills`:

```bash
/plugin marketplace add cucurb-it/analysis-gated-workflow-skills
/plugin install analysis-gated-workflow@analysis-gated-workflow-skills
```

### Local installation

Copy the `skills/` folder to your project or user skills directory:

```bash
# Project-level (shared via version control)
cp -r skills/ .claude/skills/

# User-level (available in all your Claude Code sessions)
cp -r skills/ ~/.claude/skills/
```

### Auto-install for your team

Add to `.claude/settings.json` in your project repository so the plugin
installs automatically when team members trust the project folder:

```json
{
  "extraKnownMarketplaces": {
    "analysis-gated-workflow": {
      "source": {
        "source": "github",
        "repo": "cucurb-it/analysis-gated-workflow-skills"
      }
    }
  }
}
```

## How to Start

Load the Governing Workflow skill at the start of every session:

```
Read skills/00-governing-workflow/SKILL.md and follow its Session Start Protocol.
```

The Governing Workflow skill handles everything from there — new session or resume.

---

## Core Principles

- The ANALYSIS document is the sole persistent memory of the workflow
- The Architect is the only authority who can advance a phase
- The codebase is never touched before Phase 05
- ADRs are permanent — never overwritten or deleted
- Every user prompt is logged verbatim in the ANALYSIS document

---

## Phase Overview

```
PHASE 01 — Deep Feature Analysis
  Understand what is requested. No code access. No implementation.
       ↓ [Architect go signal]
PHASE 02 — Deep Code Analysis
  Understand the existing implementation deeply. No implementation.
       ↓ [Architect go signal]
PHASE 03 — Compliance & Review  ←──────────────────┐
  Iterate on analysis based on Architect ADR        │
  annotations until analysis is complete.           │
       ↓ [Architect go signal]          [ADR added — loop back]
PHASE 04 — Implementation Planning
  Produce a detailed, phase-structured Implementation Plan. No code.
       ↓ [Architect go signal]
PHASE 05a — Implementation
  Execute the Implementation Plan, phase by phase.
       ↓ [Architect accepts]        [Architect rejects → PHASE 05b]
PHASE 05b — Rejection & Revert ──────────────────────┐
  Revert codebase. Document learnings.                │
  Update Implementation Plan.                         │
       ↓ [Architect go signal] ──────────────────────→ back to PHASE 05a
PHASE 06 — Documentation & Cleanup
  Document implementation, deviations, and results.
       ↓ [Workflow closed]
```
