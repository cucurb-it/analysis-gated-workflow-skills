---
name: agw-deep-feature-analysis
description: Phase 01 of the Analysis-Gated Workflow. Use when capturing and structuring a deep understanding of a requested feature or refactoring, based on Architect and user input. No codebase access. No implementation.
---

# Analysis-Gated Workflow — Deep Feature Analysis Skill
## DEEP FEATURE ANALYSIS PHASE

> File paths, frontmatter, anchors, and the navigation bar in this skill follow the
> **Bundle & File Naming Conventions** defined in `skills/00-governing-workflow/SKILL.md`.
> Every `phase.md` and `summary.md` carries the navigation bar directly under its H1 title;
> `doc-*.md` and ADR files do not. For this phase,
> `{{PHASE_DIR}}` = `{{FOLDER_NAME}}/phase-01-deep-feature-analysis`, so
> `{{PHASE_FILE}}` = `{{PHASE_DIR}}/phase.md` and `{{SUMMARY_FILE}}` = `{{PHASE_DIR}}/summary.md`.

---

## Purpose

This skill governs the Deep Feature Analysis Phase of the Analysis-Gated Workflow. Its sole responsibility
is to capture, structure, and document a deep understanding of **what is being requested**
— the feature, refactoring, or capability under analysis — based entirely on information
provided by the Architect and user.

No codebase access occurs in this phase.
No implementation is proposed in this phase.
No assumptions about the existing code are made in this phase.

---

## Inputs

At the start of Phase 01, the following information has been collected by the
Governing Workflow skill:

- `{{FEATURE_NAME}}` — the name of the feature or refactoring
- `{{FOLDER_NAME}}` — the working folder (bundle root) for this analysis
- Specifics provided by the user: class name, pattern, business capability,
  service contract, domain name, or similar
- The user's description of the required feature or expected refactoring

If any of these inputs are missing or ambiguous, ask clarifying questions
**before writing anything to `{{PHASE_FILE}}`**.

---

## Phase 01 Execution

### Step 1 — Log the prompt

Append the initiating prompt to `log.md`, verbatim, before doing anything else
(see the Prompt Log section of the governing skill).

### Step 2 — Analyse the request

Before writing, reason carefully about the following:

- What is being asked — in precise technical terms
- What domain or business capability is involved
- What the scope appears to be — what is included and what is explicitly or implicitly excluded
- What terminology the user is using — preserve it exactly, do not substitute your own
- What ambiguities or gaps exist in the description

### Step 3 — Write to the phase file

Write `{{PHASE_FILE}}` with frontmatter (`type: Phase`, `phase: "01 — Deep Feature
Analysis"`, `status: IN PROGRESS`, one-line `description`). Emit the common anchors
(`phase-top`, `outcome`, `open-items`) and the phase-specific anchor
(`feature-description`) at their sections. Use plain `###` subsection headings — do
**not** number them (the old `N.x` numbering was an artifact of the single-document
model and produces orphaned numbers in a per-phase file). Structure:

```markdown
---
type: Phase
phase: "01 — Deep Feature Analysis"
status: IN PROGRESS
description: <one-line summary>
---

<a id="phase-top"></a>
# [FEATURE_NAME] — Deep Feature Analysis

[Index](../index.md) · [State](../STATE.md) · [Log](../log.md)

---

<a id="feature-description"></a>
## Feature or Refactoring Description

### Request Summary
### Domain Context
### Scope
### Key Concepts & Terminology

<a id="open-items"></a>
## Open Questions

<a id="outcome"></a>
## Outcome
```

Populate each subsection:

**Request Summary** — A precise restatement of what has been requested, in your own words.
This is not a copy of the user's prompt — it is your structured understanding of it.

**Domain Context** — The business or technical domain this feature or refactoring belongs to.
What capability it serves. What problem it solves or what improvement it delivers.

**Scope** — What is in scope. What is explicitly out of scope. What is unclear and requires
clarification before proceeding.

**Key Concepts & Terminology** — The domain-specific terms, class names, patterns, or
concepts identified in the user's description. Defined as the user uses them — not as
general programming terms.

**Open Questions** (under the `open-items` anchor) — Any ambiguities, missing information,
or assumptions that need Architect confirmation before Phase 02 can begin. If there are
none, state that explicitly.

**Outcome** (under the `outcome` anchor) — A one-to-three sentence statement of what this
phase established: the structured understanding of the request that Phase 02 will build on.

### Step 4 — Write the summary file

Write `{{SUMMARY_FILE}}` (`type: Summary`, `phase: "01 — Deep Feature Analysis"`,
`status: AWAITING ARCHITECT REVIEW`, `phase_file: ./phase.md`, one-line `description`).
Keep it compact — work completed, outcome, and open items only, each linking back into
`{{PHASE_FILE}}`'s anchors:

```markdown
---
type: Summary
phase: "01 — Deep Feature Analysis"
status: AWAITING ARCHITECT REVIEW
phase_file: ./phase.md
description: <one-line outcome>
---

# Phase 01 Summary — Deep Feature Analysis

[Index](../index.md) · [State](../STATE.md) · [Log](../log.md)

---

**Work completed:** [one or two lines]

**Outcome:** [one line] → [full detail](./phase.md#outcome)

**Open items carried forward:** [list, or "none"] → [detail](./phase.md#open-items)
```

### Step 5 — Update workflow state

Update `{{FOLDER_NAME}}/STATE.md`:
- `Phase Status` → `AWAITING ARCHITECT REVIEW`
- `Pending Architect Action` → `Review Phase 01 output. Signal readiness for Phase 02.`

### Step 6 — Notify the Architect

Inform the Architect that the Deep Feature Analysis Phase is complete. Do not summarise the phase file
in chat — direct the Architect to read the summary. State clearly what action is expected:
> "Deep Feature Analysis Phase is complete. Please review `phase-01-deep-feature-analysis/summary.md`
> and signal when you are ready to proceed to Deep Code Analysis Phase, or add annotations if
> corrections are needed."

---

## What Phase 01 Does NOT Do

- Does not access the codebase
- Does not propose a solution or approach
- Does not describe how the feature will be implemented
- Does not make assumptions about the existing code structure
- Does not advance to Phase 02 without an explicit Architect signal

---

## Quality Criteria

Phase 01 output is complete when:

- [ ] The request is restated in precise technical terms
- [ ] The domain context is documented
- [ ] Scope boundaries are explicit — including what is unclear
- [ ] All key terminology from the user is captured and defined
- [ ] Open questions are listed, or explicitly stated as none
- [ ] `{{PHASE_FILE}}` emits the required anchors
- [ ] `{{SUMMARY_FILE}}` is written and links back to the phase file
- [ ] `STATE.md` reflects `AWAITING ARCHITECT REVIEW`
