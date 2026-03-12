# Analysis-Gated Workflow — Skill Set

A structured, phase-gated workflow for software development and refactoring,
governed by a Software Architect and supported by AI (Claude Code CLI or Copilot CLI).

---

## Skills

| Folder | Skill | Phase |
|---|---|---|
| `00-governing-workflow/` | Governing Workflow | All phases — orchestrator |
| `01-deep-feature-analysis/` | Deep Feature Analysis | DEEP FEATURE ANALYSIS PHASE |
| `02-deep-code-analysis/` | Deep Code Analysis | DEEP CODE ANALYSIS PHASE |
| `03-compliance-review/` | Compliance & Review | COMPLIANCE & REVIEW PHASE (iterative) |
| `04-implementation-planning/` | Implementation Planning | IMPLEMENTATION PLANNING PHASE |
| `05-implementation/` | Implementation | IMPLEMENTATION PHASE & DOCUMENTATION |

---

## Installation

### Via Claude Code CLI (recommended)

Once published to GitHub at `cucurb-it/analysis-gated-workflow-skills`:

```bash
/plugin marketplace add https://github.com/cucurb-it/analysis-gated-workflow-skills.git
/plugin install analysis-gated-workflow@analysis-gated-workflow-skills
```

### Via GitHub Copilot CLI

```bash
# Add the marketplace
gh copilot skill marketplace add https://github.com/cucurb-it/analysis-gated-workflow-skills.git

# Install the skill
gh copilot skill install analysis-gated-workflow
```

### Via VS Code Extension (easiest for VS Code users)

Install the [Agent Plugins extension](https://github.com/timheuer/vscode-agent-plugins) for VS Code, which provides a UI for managing marketplaces and skills:

1. Install the extension from the VS Code marketplace
2. Open the Command Palette (`Ctrl+Shift+P` / `Cmd+Shift+P`)
3. Run: `Agent Plugins: Add Marketplace`
4. Enter: `https://github.com/cucurb-it/analysis-gated-workflow-skills.git`
5. Install the skill scoped to your workspace or user settings

The extension makes it easy to browse, install, and manage skills without command-line tools.

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
- The codebase is never touched before IMPLEMENTATION PHASE
- ADRs are permanent — never overwritten or deleted
- Every user prompt is logged verbatim in the ANALYSIS document

---

## Phase Overview

```
DEEP FEATURE ANALYSIS PHASE
  Understand what is requested. No code access. No implementation.
       ↓ [Architect go signal]
DEEP CODE ANALYSIS PHASE
  Understand the existing implementation deeply. No implementation.
       ↓ [Architect go signal]
COMPLIANCE & REVIEW PHASE  ←──────────────────┐
  Iterate on analysis based on Architect ADR   │
  annotations until analysis is complete.      │
       ↓ [Architect go signal]    [ADR added — loop back]
IMPLEMENTATION PLANNING PHASE
  Produce a detailed, phase-structured Implementation Plan. No code.
       ↓ [Architect go signal]
IMPLEMENTATION PHASE
  Execute the Implementation Plan, phase by phase.
       ↓ [Architect accepts]        [Architect rejects → REJECTION & REVERT]
REJECTION & REVERT ──────────────────────────┐
  Revert codebase. Document learnings.        │
  Update Implementation Plan.                 │
       ↓ [Architect go signal] ───────────────→ back to IMPLEMENTATION PHASE
DOCUMENTATION & CLEANUP
  Document implementation, deviations, and results.
       ↓ [Workflow closed]
```