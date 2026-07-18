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
| `05-implementation/` | Implementation | IMPLEMENTATION PHASE (incl. rejection & revert) |
| `06-documentation/` | Documentation & Cleanup | DOCUMENTATION & CLEANUP PHASE |

Each workflow run produces an **analysis bundle** — a folder of markdown files rather than
a single document. See [Core Principles](#core-principles) for the layout and the reasoning
behind it.

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

### The analysis bundle

The **analysis bundle** is the persistent memory of a workflow run — a single folder,
one per feature or refactoring, holding the entire analysis as a set of small markdown
files rather than one growing document. Its shape:

```
{{FEATURE_FOLDER}}/
├── index.md                              # regenerable root listing — links + descriptions, no live state
├── STATE.md                              # live workflow state: current phase, status, pending action
├── log.md                                # every user prompt, verbatim, date-grouped newest-first
├── phase-01-deep-feature-analysis/
│   ├── phase.md                          # full analysis content for the phase
│   └── summary.md                        # compact "what happened / outcome / open items", sorts last
├── phase-02-deep-code-analysis/
│   ├── phase.md
│   └── summary.md
├── phase-03-compliance-review/
│   ├── phase.md
│   └── summary.md
├── phase-04-implementation-planning/
│   ├── phase.md
│   └── summary.md
├── phase-05-implementation/
│   ├── phase.md
│   └── summary.md
├── phase-06-documentation/
│   ├── phase.md                          # thin working notes
│   ├── summary.md                        # the bundle's front-door summary
│   ├── doc-executive.md                  # audience seed: business-outcome brief
│   ├── doc-business.md                   # audience seed: business process & experience
│   └── doc-technical.md                  # audience seed: technical record
└── adrs/
    └── YYYY-MM-DD-<slug>.md              # one file per Architecture Decision Record
```

### Why a bundle instead of one document

A single analysis document grows without bound. By the implementation phase it carries
feature analysis, code analysis, compliance iterations, ADRs, the implementation plan,
deviations, and the prompt log — all at once. Two people pay for that: the AI reloads the
whole thing into context every session, and a developer wanting one fact has to scroll
past everything else to find it.

The core insight driving the layout: **not everyone needs all the information all the
time.** Splitting the document by phase, and giving every phase a compact `summary.md`
alongside its full `phase.md`, lets each reader load only what their task needs:

| Reader / task | Reads |
|---|---|
| Resuming a session | `STATE.md` + current phase's `summary.md` |
| Checking a decision | the relevant file in `adrs/` |
| Reviewing what a phase concluded | that phase's `summary.md` |
| Needing full detail on a phase | that phase's `phase.md` |
| Onboarding to the whole feature | `index.md`, then drill down |
| Auditing the conversation | `log.md` |

`summary.md` is bounded in size no matter how large `phase.md` grows, so orientation stays
cheap even when the underlying analysis is deep. It links back into `phase.md` via stable
anchors, so "give me the detail" is always one click away.

### Close-out: three audience documents

The final phase (Documentation & Cleanup) closes a feature by producing three
audience-targeted documents in `phase-06-documentation/`, one for each typical audience of
a software project:

| File | Audience | Substance |
|---|---|---|
| `doc-executive.md` | Executive / sponsor | Business-outcome brief — capability delivered, risk delta, what it unblocks. TCO is out of scope. |
| `doc-business.md` | Business stakeholder | The end-to-end business process the feature enables; UI/UX described as *where that process surfaces*. |
| `doc-technical.md` | Development team | The technical record — files changed, deviations, learnings, measured results. |

All three are produced for every feature, however briefly, so consumers always know where
to look. They are **expansion seeds, not finished documents**: terse bullets under fixed
headings, marked `expandable: true`. They are never shared verbatim outside the development
team — an external reader always receives a redacted and/or expanded version generated from
the seed. Writing the same work for three audiences also surfaces gaps: if the business or
executive seed can't be filled, the feature's *purpose* wasn't fully understood, only its
mechanics.

### Format lineage — OKF-shaped, not OKF-conformant

The bundle layout is **inspired by the Open Knowledge Format (OKF)**, an open,
vendor-neutral specification published by Google Cloud (v0.1, June 2026) for representing
knowledge as a directory of markdown files with YAML frontmatter. In OKF, each file is one
"concept," its path is the concept's identity, files cross-link with ordinary markdown
links to form a graph, and the spec is deliberately minimal: **exactly one frontmatter
field is required — `type`** — with everything else (other fields, filenames, body
structure) left to the producer. There's no runtime, SDK, or database; a bundle is "just
files."

We borrow that shape because it happens to solve our readability problem well:
per-concept files, YAML frontmatter, bundle-relative cross-links, and a regenerable
`index.md`. Each of our files carries `type`, and we add our own workflow fields
(`phase`, `status`, `phase_file`, …) — which OKF explicitly permits, since it constrains
only `type`.

But we make **no claim to be OKF-conformant**, and the distinction is worth being precise
about. OKF exists to make knowledge *interoperable across producers and consumers* — one
team's wiki readable by another team's agent without translation. Our bundle has the
opposite lifespan and audience:

| | OKF | This workflow's bundle |
|---|---|---|
| Purpose | Durable, shared organizational knowledge | Single-feature workflow state |
| Lifespan | Long-lived, curated, updated over time | Created per feature, disposable after merge |
| Audience | Any agent or tool, across orgs | This workflow, this Architect, this run |
| "Concepts" | Tables, metrics, APIs, runbooks | Phases, summaries, decisions, live state |
| Interoperability | The whole point | A non-goal |

So we treat OKF as a **shape to borrow, not a standard to meet**. Files like `STATE.md`
and `log.md` are workflow control and transcript files, not curated concepts, and we don't
contort them to fit a knowledge-base model. If a future need arises to hand a completed
bundle to another OKF-aware tool, the gap to conformance is small — but closing it is not
a goal of this project today.

> Note: "OKF" is also the abbreviation of the **Open Knowledge Foundation**, an unrelated
> non-profit. Here it always means the file-format specification.

### Governing rules

- **The Architect is the only authority who can advance a phase.** The AI never
  self-initiates a transition; it stops at every gate and waits for an explicit signal.
- **The codebase is never touched before the IMPLEMENTATION PHASE.** Phases 01–04 build
  understanding and a plan; no file is created, edited, or deleted until Phase 05.
- **ADRs are permanent.** Each is one file under `adrs/`, never overwritten, summarised
  away, or deleted. Compliance responses are appended inside the same file, so a decision
  and its follow-through stay together and accumulate across iterations.
- **Every user prompt is logged verbatim in `log.md`**, before any response is written —
  the log is the workflow's audit trail.
- **`STATE.md` is the single source of live state**, and `index.md` is regenerable from
  frontmatter — live state never lives in the regenerable file.
- **Every session ends with `STATE.md` and the current phase's `summary.md` updated** to
  reflect current reality, so the next session (human or AI) can resume cleanly.

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