---
name: agw-implementation
description: the Implementation Phase of the Analysis-Gated Workflow. Use when executing an approved Implementation Plan, and handling rejection and revert cycles. Documentation of the completed work is handled by the separate Documentation & Cleanup skill (Phase 06).
---

# Analysis-Gated Workflow — Implementation Skill
## IMPLEMENTATION PHASE

> File paths, frontmatter, anchors, and the navigation bar in this skill follow the
> **Bundle & File Naming Conventions** defined in `skills/00-governing-workflow/SKILL.md`.
> Every `phase.md` and `summary.md` carries the navigation bar directly under its H1 title;
> `doc-*.md` and ADR files do not.
> Phase 05 writes to `{{FOLDER_NAME}}/phase-05-implementation/` (`phase.md`, `summary.md`).
> When the Architect accepts the implementation, the workflow advances to the separate
> **Documentation & Cleanup skill** (`skills/06-documentation/SKILL.md`).

---

## Purpose

This skill governs the Implementation Phase of the Analysis-Gated Workflow.
Implementation Phase is the first phase in which the codebase is touched. The Implementation
Plan produced in Phase 04 is the sole guide for all changes made.

The Architect retains full authority to reject the implementation at any point.
A rejection triggers Phase 05b — revert, learn, and re-enter. Once the Architect
**accepts** the implementation, the workflow advances to Phase 06 (Documentation &
Cleanup), governed by its own skill.

---

## Inputs

- The completed analysis bundle, including the Implementation Plan in `phase-04-implementation-planning/phase.md`
- Access to the codebase via Claude Code CLI or Copilot CLI
- The source code repository (for revert capability in Phase 05b)

---

## Implementation Constraints

These constraints apply throughout Phase 05 and cannot be overridden by prompt wording:

- **Simplicity First** — make every change as simple as possible. Minimize code impact.
  Choose the simplest solution that satisfies the requirements. Avoid over-engineering.
  Prefer small, focused changes over large refactorings unless explicitly required by the
  Implementation Plan.
- **No Laziness** — identify root causes for the requested feature or refactoring.
  Do not apply superficial fixes or workarounds. Understand why the change is needed and
  address the underlying problem. If the Implementation Plan is based on symptoms rather
  than root cause, flag it as a deviation and return to analysis.
- **Avoid Temporary Fixes** — do not introduce TODO comments, placeholder implementations,
  or "we'll fix this later" code. Every change must be production-ready. If a proper
  solution cannot be implemented now, flag it as a deviation and discuss with the Architect.
- **Senior Developer Standards** — apply the code quality, testing rigor, and engineering
  discipline expected of a senior developer. Write code you would be proud to have reviewed
  by your peers. Consider maintainability, readability, and long-term consequences.
- **Minimal Impact** — touch only what is necessary to satisfy the Implementation Plan.
  Do not refactor unrelated code, rename variables "for consistency", or apply coding
  standards to files you're not already modifying. Every line changed increases the risk
  of introducing new bugs. Avoid scope creep.
- **No new dependencies** — do not introduce new NuGet packages or external libraries
  without explicit Architect approval. If a dependency appears necessary, stop and raise
  it as a deviation before proceeding.
- **No unnecessary comments** — do not add explanatory comments to code unless they
  were present in the original or are explicitly required by the Implementation Plan.
- **Null safety is critical** — every new or modified code path must be reviewed for
  null dereference risk before moving on. Null-related bugs are the most common source
  of regression. When in doubt, add a null check.
- **Continuous validation** — do not wait until the end of a phase to build and check.
  After every meaningful code change, verify the build is clean and no new warnings
  have been introduced.

---

## Phase 05a — Implementation Execution

For this phase, `{{PHASE_FILE}}` = `{{FOLDER_NAME}}/phase-05-implementation/phase.md`
and `{{SUMMARY_FILE}}` = `{{FOLDER_NAME}}/phase-05-implementation/summary.md`.
Initialise `{{PHASE_FILE}}` with frontmatter (`type: Phase`, `phase: "05 — Implementation"`,
`status: IN PROGRESS`, one-line `description`) and emit the common anchors plus the
phase-specific anchors `deviations` and `implementation-summary`.

### Step 1 — Log the prompt

Append the Phase 05 initiating prompt to `log.md`, verbatim.

### Step 2 — Confirm the Implementation Plan

Before writing a single line of code, complete the following confirmations:

#### 2.1 — Re-read project conventions in great detail

The `.claude/` and `.github/` folders contain the authoritative rules for this codebase.
**Re-read every .md file thoroughly** to refresh understanding before touching any code:

- `.claude/CLAUDE.md` and all other `.md` files — read everything deeply
- `.github/` folder — read all `.md` files, understand all coding standards and workflows

Create a **Project Conventions Re-Confirmation** section in `{{PHASE_FILE}}`:

```markdown
### Project Conventions Re-Confirmation (Phase 05)

#### `.claude/CLAUDE.md`
**Re-Read:** ✓  
**CRITICAL/MANDATORY Rules Impacting This Implementation:**
- [Any CRITICAL/MANDATORY rules that apply to files being modified — extract verbatim]

**Other Rules Impacting This Implementation:**
- [Architecture boundaries that constrain this change]
- [Technology constraints that apply to files being modified]
- [Naming conventions to follow]
- [Critical workflows this implementation must respect]
- [Hard constraints that cannot be violated]

#### `.github/github-instructions.md` (or similar)
**Re-Read:** ✓  
**CRITICAL/MANDATORY Rules Impacting This Implementation:**
- [extract if applicable]

**Other Rules Impacting This Implementation:**
- [List only rules directly relevant to the Implementation Plan]

#### `instructions/csharp-instructions.md` (or similar)
**Re-Read:** ✓  
**CRITICAL/MANDATORY Rules Impacting This Implementation:**
- [extract if applicable]

**Other Rules Impacting This Implementation:**
- [List only rules directly relevant to the Implementation Plan]

[Repeat for each .md file]
```

Focus on rules that directly affect the Implementation Plan being executed.
This is not a full re-extraction — it's a targeted confirmation of rules that
matter for the specific code being written.

**CRITICAL**: Any rule marked CRITICAL, MANDATORY, IMPORTANT, DO NOT, NEVER, ALWAYS
that affects the code being written MUST be listed verbatim. These are non-negotiable
constraints on the implementation.

#### 2.2 — Review the Implementation Plan

Re-read the Implementation Plan in `phase-04-implementation-planning/phase.md`.

Confirm with the Architect:

> "I am about to begin implementation following the approved plan. I will execute
> Phase 1 first: [brief description]. I will work through all phases and tasks without
> stopping, marking each item complete as I go. Please confirm I should proceed."

Wait for explicit confirmation.

### Step 3 — Execute to completion

Execute the entire Implementation Plan without stopping between phases, unless:
- A deviation from the plan is encountered (invoke Deviation Protocol)
- A build error cannot be resolved without Architect input
- A null safety risk is identified that the plan does not address

For each checklist item:
1. Implement it
2. Validate — build clean, no new warnings, null safety reviewed
3. Tick it in the Implementation Plan: `- [x]`
4. Proceed immediately to the next item

At the end of each phase, report progress to the Architect and continue to the
next phase without waiting for acknowledgement, unless a gate condition above is met.

**Do not skip checklist items.** If an item cannot be completed as written, stop
immediately and invoke the Deviation Protocol.

### Step 4 — Deviation Protocol

If at any point the implementation requires deviating from the approved plan:

1. **Stop immediately** — do not continue implementing
2. Document the deviation in `{{PHASE_FILE}}` under `<a id="deviations"></a> ## Deviations`:

```markdown
### Deviation [N] — YYYY-MM-DD

|     | Info |
| --- | --- |
| **Planned** | [What the Implementation Plan specified] |
| **Actual** | [What was actually implemented or discovered] |
| **Reason** | [Root cause — why the deviation was necessary] |
| **Impact** | [What this changes about the remaining plan] |
| **Decision** | PENDING |
```

3. Notify the Architect:
   > "I have encountered a deviation from the Implementation Plan. I have stopped
   > implementation and documented it under the Deviations section of
   > `phase-05-implementation/phase.md`. Please review and signal how to proceed."

4. Wait for Architect decision before continuing.

5. When the Architect decides, update the deviation entry Decision field:
   - `APPROVED — [rationale]` → continue with the deviation
   - `REJECTED — [rationale]` → revert to the plan as written

**Example deviation:**

```markdown
### Deviation 1 — 2026-03-10

|     | Info |
| --- | --- |
| **Planned** | Conditional thinking block rendered inside the bot message `<Padder>` using `@if (!string.IsNullOrEmpty(message.ThinkingContent))` as a child of `<Padder>`. |
| **Actual** | `<Padder>` in RazorConsole does not support dynamic child counts. Placing `@if` blocks inside `<Padder>` (including the `@if (message.IsUser)` branch) caused the component to fail to render any children — neither the thinking block nor the response block appeared. |
| **Reason** | The original codebase establishes a clear convention: `<Rows>` handles dynamic/conditional children; `<Padder>` always receives a fixed set of children. The Implementation Plan specified the template structure but did not account for this RazorConsole-specific constraint. |
| **Impact** | Phase 3.2 (rendering loop) must be restructured. Conditional content must live at the `<Rows>` level. Each `<Padder>` must have exactly 3 fixed children. Two separate `<Padder>` instances are used for bot messages: one conditional Padder for thinking content, one always-present Padder for the response. |
| **Decision** | APPROVED — self-evident fix from the existing codebase convention. |
```

### Step 5 — Update workflow state after each phase

After each implementation phase is complete, update `{{FOLDER_NAME}}/STATE.md`:
- `Phase Status` → `IN PROGRESS — Implementation Phase [N] complete`
- `Last Updated` → today's date
- `Pending Architect Action` → `Review Phase [N] output. Signal to proceed to Phase [N+1].`

### Step 6 — On acceptance, hand off to Phase 06

When all implementation phases are complete, write `{{SUMMARY_FILE}}`
(`{{FOLDER_NAME}}/phase-05-implementation/summary.md`) — work completed, outcome, and any
deviations, linking back into `{{PHASE_FILE}}`'s anchors — then notify the Architect:

> "Implementation is complete. Please review and either accept, or reject (which triggers
> Rejection & Revert). On acceptance, the workflow advances to Documentation & Cleanup."

On the Architect's **acceptance** signal, the Governing Workflow advances to Phase 06,
governed by `skills/06-documentation/SKILL.md`. This skill does not perform documentation itself.

---

## Phase 05b — Rejection & Revert

If the Architect rejects the implementation:

### Step 1 — Stop all implementation activity immediately

Do not make further changes to the codebase.

### Step 2 — Document the rejection

Add to `{{PHASE_FILE}}` under `## Implementation Iterations`:

```markdown
### Implementation Iteration [N] — REJECTED [YYYY-MM-DD]
**Rejection reason:** [Architect's stated reason, verbatim]
**Reverted:** [YYYY-MM-DD]
```

### Step 3 — Revert the codebase

The changed files are reverted via the source code repository.
Confirm with the Architect that the revert is complete before proceeding.

### Step 4 — Extract learnings

Analyse what went wrong and document it under the same section:

```markdown
### Learnings from Iteration [N]
- [Learning 1 — what assumption was wrong, what was missed, what should change]
- [Learning 2]
- ...
```

### Step 5 — Update the Implementation Plan

Based on the learnings, propose updates to the Implementation Plan that address
the rejection reason. These updates are treated as ADRs — the Architect must
review and approve the updated plan before Phase 05a re-enters.

### Step 6 — Update workflow state

Update `{{FOLDER_NAME}}/STATE.md`:
- `Phase Status` → `AWAITING ARCHITECT REVIEW`
- `Pending Architect Action` → `Review updated Implementation Plan. Signal go for next implementation iteration.`

---

## What This Skill Does NOT Do

- Does not begin implementation without explicit Architect go signal
- Does not skip checklist items silently
- Does not continue past a deviation without Architect decision
- Does not self-accept an implementation — only the Architect can accept
- Does not perform documentation & cleanup — that is Phase 06's separate skill

---

## Quality Criteria

### Phase 05a complete when:
- [ ] Project conventions re-read and re-confirmed before implementation started
- [ ] All Implementation Plan checklist items are ticked
- [ ] All deviations are documented with Architect decisions
- [ ] Each phase ended with a build-and-validate step
- [ ] `STATE.md` is updated after each phase

### Phase 05b complete when:
- [ ] Rejection is documented with Architect's reason verbatim
- [ ] Revert is confirmed
- [ ] Learnings are documented
- [ ] Updated Implementation Plan is approved by Architect
