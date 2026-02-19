---
name: agw-implementation
description: the Implementation Phase and Documentation phase of the Analysis-Gated Workflow. Use when executing an approved Implementation Plan, handling rejection and revert cycles, and documenting the completed implementation with deviations and learnings.
---

# Analysis-Gated Workflow — Implementation Skill
## IMPLEMENTATION PHASE & DOCUMENTATION

---

## Purpose

This skill governs the Implementation Phase and Documentation phase of the Analysis-Gated Workflow. Implementation Phase is the
first phase in which the codebase is touched. The Implementation Plan produced in
Phase 04 is the sole guide for all changes made. Phase 06 documents the completed
implementation and all deviations from the plan.

The Architect retains full authority to reject the implementation at any point.
A rejection triggers Phase 05b — revert, learn, and re-enter.

---

## Inputs

- The completed ANALYSIS document including the Implementation Plan
- Access to the codebase via Claude Code CLI or Copilot CLI
- The source code repository (for revert capability in Phase 05b)

---

## Implementation Constraints

These constraints apply throughout Phase 05 and cannot be overridden by prompt wording:

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

### Step 1 — Log the prompt

Append the Phase 05 initiating prompt to the `## Prompt Log` section, verbatim.

### Step 2 — Confirm the Implementation Plan

Before writing a single line of code, complete the following confirmations:

#### 2.1 — Re-read project conventions in great detail

The `.claude/` and `.github/` folders contain the authoritative rules for this codebase.
**Re-read every .md file thoroughly** to refresh understanding before touching any code:

- `.claude/CLAUDE.md` and all other `.md` files — read everything deeply
- `.github/` folder — read all `.md` files, understand all coding standards and workflows

Create a **Project Conventions Re-Confirmation** section in the ANALYSIS document:

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

Re-read the `## Implementation Plan` section of the ANALYSIS document.

Confirm with the Architect:

> "I am about to begin implementation following the plan in the ANALYSIS document.
> I will execute Phase 1 first: [brief description]. I will work through all phases
> and tasks without stopping, marking each item complete as I go.
> Please confirm I should proceed."

Wait for explicit confirmation.

### Step 3 — Execute to completion

Execute the entire Implementation Plan without stopping between phases, unless:
- A deviation from the plan is encountered (invoke Deviation Protocol)
- A build error cannot be resolved without Architect input
- A null safety risk is identified that the plan does not address

For each checklist item:
1. Implement it
2. Validate — build clean, no new warnings, null safety reviewed
3. Tick it in the ANALYSIS document: `- [x]`
4. Proceed immediately to the next item

At the end of each phase, report progress to the Architect and continue to the
next phase without waiting for acknowledgement, unless a gate condition above is met.

**Do not skip checklist items.** If an item cannot be completed as written, stop
immediately and invoke the Deviation Protocol.

### Step 4 — Deviation Protocol

If at any point the implementation requires deviating from the approved plan:

1. **Stop immediately** — do not continue implementing
2. Document the deviation in the ANALYSIS document:

```
### Deviation [N] — [YYYY-MM-DD]
**Planned:** [What the plan said]
**Actual:** [What was found or needed instead]
**Reason:** [Why the deviation is necessary]
**Impact:** [What this changes about the remaining plan]
**Architect decision:** [PENDING]
```

3. Notify the Architect:
   > "I have encountered a deviation from the Implementation Plan. I have stopped
   > implementation and documented it in the ANALYSIS document under ## Deviations.
   > Please review and signal how to proceed."

4. Wait for Architect decision before continuing.

5. When the Architect decides, update the deviation entry:
   - `**Architect decision:** APPROVED — [rationale]` → continue with the deviation
   - `**Architect decision:** REJECTED — [rationale]` → revert to the plan as written

### Step 5 — Update Workflow State after each phase

After each implementation phase is complete:

```
Current Phase:           PHASE 05
Phase Status:            IN PROGRESS — Implementation Phase [N] complete
Last Updated:            YYYY-MM-DD
Pending Architect Action: Review Phase [N] output. Signal to proceed to Phase [N+1].
```

---

## Phase 05b — Rejection & Revert

If the Architect rejects the implementation:

### Step 1 — Stop all implementation activity immediately

Do not make further changes to the codebase.

### Step 2 — Document the rejection

Add to the ANALYSIS document under `## Implementation Iterations`:

```
### Implementation Iteration [N] — REJECTED [YYYY-MM-DD]
**Rejection reason:** [Architect's stated reason, verbatim]
**Reverted:** [YYYY-MM-DD]
```

### Step 3 — Revert the codebase

The changed files are reverted via the source code repository.
Confirm with the Architect that the revert is complete before proceeding.

### Step 4 — Extract learnings

Analyse what went wrong and document it:

```
### Learnings from Iteration [N]
- [Learning 1 — what assumption was wrong, what was missed, what should change]
- [Learning 2]
- ...
```

### Step 5 — Update the Implementation Plan

Based on the learnings, propose updates to the Implementation Plan that address
the rejection reason. These updates are treated as ADRs — the Architect must
review and approve the updated plan before Phase 05a re-enters.

### Step 6 — Update Workflow State

```
Current Phase:           PHASE 05b
Phase Status:            AWAITING ARCHITECT REVIEW
Last Updated:            YYYY-MM-DD
Pending Architect Action: Review updated Implementation Plan.
                          Signal go for next implementation iteration.
```

---

## Phase 06 — Documentation & Cleanup

Documentation & Cleanup phase is entered after the Architect accepts the implementation.

### Step 1 — Log the prompt

Append the Phase 06 initiating prompt to the `## Prompt Log` section, verbatim.

### Step 2 — Mark all completed checklist items

Verify every checklist item in the Implementation Plan is correctly ticked.
Any item that was not executed must be explicitly noted as `[SKIPPED — reason]`.

### Step 3 — Populate Implementation Summary

Populate the `## Implementation Summary` section of the ANALYSIS document:

#### Files Created
| File | Description |
|---|---|
| `path/to/file` | What it contains and why it was created |

#### Files Modified
| File | Changes |
|---|---|
| `path/to/file` | What was changed and why |

#### Files Deleted
| File | Reason |
|---|---|
| `path/to/file` | Why it was removed |

#### Deviations from Plan
List all deviations with their approved rationale. If none: state explicitly.

#### Learnings
Any insights gained during implementation that are relevant to future work
on this codebase — patterns discovered, constraints encountered, pitfalls avoided.

### Step 4 — Add Results (if performance or behaviour was measured)

If the implementation addressed a measurable problem (e.g. performance):

#### Before / After Comparison
| Metric | Before | After | Improvement |
|---|---|---|---|
| [Metric] | [value] | [value] | [delta] |

Note: leave this section as `[pending developer measurement]` if results are
not yet available. It must be completed before the ANALYSIS document is closed.

### Step 5 — Final Workflow State

```
Current Phase:           PHASE 06
Phase Status:            COMPLETE
Last Updated:            YYYY-MM-DD
Pending Architect Action: none
```

### Step 6 — Close the workflow

> "Documentation & Cleanup phase is complete. The ANALYSIS document is fully updated. The
> Analysis-Gated Workflow for [FEATURE_NAME] is closed."

---

## What This Skill Does NOT Do

- Does not begin implementation without explicit Architect go signal
- Does not skip checklist items silently
- Does not continue past a deviation without Architect decision
- Does not self-accept an implementation — only the Architect can accept
- Does not close the workflow before the Implementation Summary is complete

---

## Quality Criteria

### Phase 05a complete when:
- [ ] Project conventions re-read and re-confirmed before implementation started
- [ ] All Implementation Plan checklist items are ticked
- [ ] All deviations are documented with Architect decisions
- [ ] Each phase ended with a build-and-validate step
- [ ] Workflow State is updated after each phase

### Phase 05b complete when:
- [ ] Rejection is documented with Architect's reason verbatim
- [ ] Revert is confirmed
- [ ] Learnings are documented
- [ ] Updated Implementation Plan is approved by Architect

### Phase 06 complete when:
- [ ] All checklist items are ticked or marked skipped with reason
- [ ] Implementation Summary is complete (files created, modified, deleted)
- [ ] All deviations are documented
- [ ] Learnings are documented
- [ ] Before/after comparison is present (or explicitly pending measurement)
- [ ] Workflow State is `COMPLETE`