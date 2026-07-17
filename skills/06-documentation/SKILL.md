---
name: agw-documentation
description: the Documentation & Cleanup Phase of the Analysis-Gated Workflow. Use after the Architect accepts an implementation, to document the completed work, deviations, learnings, and results. No further code changes.
---

# Analysis-Gated Workflow — Documentation & Cleanup Skill
## DOCUMENTATION & CLEANUP PHASE

> File paths, frontmatter, and anchors in this skill follow the **Bundle & File Naming
> Conventions** defined in `skills/00-governing-workflow/SKILL.md`. For this phase,
> `{{PHASE_DIR}}` = `{{FOLDER_NAME}}/phase-06-documentation`, so
> `{{PHASE_FILE}}` = `{{PHASE_DIR}}/phase.md` and `{{SUMMARY_FILE}}` = `{{PHASE_DIR}}/summary.md`.

---

## Purpose

This skill governs the Documentation & Cleanup Phase of the Analysis-Gated Workflow. It is
entered **only after the Architect accepts the implementation** produced in Phase 05.

Its responsibility is to document the completed implementation — files changed, deviations
from the plan, learnings, and measured results — and to produce the bundle's front-door
summary: the file meant to be read first if someone reopens this bundle months later.

No code is written or modified in this phase.

---

## Inputs

- The accepted implementation recorded in `phase-05-implementation/phase.md`
- The Implementation Plan in `phase-04-implementation-planning/phase.md`
- All deviations recorded under `phase-05-implementation/phase.md#deviations`
- Access to the codebase (read-only) for confirming what changed

---

## DOCUMENTATION & CLEANUP PHASE Execution

Initialise `{{PHASE_FILE}}` with frontmatter (`type: Phase`, `phase: "06 — Documentation"`,
`status: IN PROGRESS`, one-line `description`) and emit the common anchors (`phase-top`,
`outcome`, `open-items`) plus the phase-specific anchor `documentation`.

### Step 1 — Log the prompt

Append the Phase 06 initiating prompt to `log.md`, verbatim.

### Step 2 — Mark all completed checklist items

Verify every checklist item in the Implementation Plan is correctly ticked.
Any item that was not executed must be explicitly noted as `[SKIPPED — reason]`.

### Step 3 — Populate the Implementation Summary

Under `<a id="documentation"></a> ## Implementation Summary` in `{{PHASE_FILE}}`:

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
Link to the deviations recorded in `phase-05-implementation/phase.md#deviations` with
their approved rationale. If none: state explicitly.

#### Learnings & Pitfalls (this feature)
Any insights gained during implementation that are relevant to future work
on this codebase — patterns discovered, constraints encountered, pitfalls avoided.
Scoped to this feature only.

### Step 4 — Add Results (if performance or behaviour was measured)

If the implementation addressed a measurable problem (e.g. performance):

#### Before / After Comparison
| Metric | Before | After | Improvement |
|---|---|---|---|
| [Metric] | [value] | [value] | [delta] |

Note: leave this as `[pending developer measurement]` if results are not yet available.
It must be completed before the bundle is closed.

Record the headline result under the `outcome` anchor, and anything still pending
(e.g. measurements) under the `open-items` anchor.

### Step 5 — Write the closing summary file

Write `{{SUMMARY_FILE}}` (`type: Summary`, `phase: "06 — Documentation"`,
`status: COMPLETE`, `phase_file: ./phase.md`, one-line `description`). This is the
bundle's front-door summary:

```markdown
---
type: Summary
phase: "06 — Documentation"
status: COMPLETE
phase_file: ./phase.md
description: <one-line feature outcome>
---

# Phase 06 Summary — Documentation & Cleanup

**Work completed:** [what was implemented, in one or two lines]

**Outcome:** [result, incl. before/after if measured] → [full detail](./phase.md#documentation)

**Deviations:** [count / "none"] → [detail](../phase-05-implementation/phase.md#deviations)
```

### Step 6 — Final workflow state

Update `{{FOLDER_NAME}}/STATE.md`:
- `Current Phase` → `DOCUMENTATION & CLEANUP PHASE`
- `Phase Status` → `COMPLETE`
- `Pending Architect Action` → `none`

### Step 7 — Close the workflow

> "Documentation & Cleanup phase is complete. The analysis bundle is fully updated. The
> Analysis-Gated Workflow for [FEATURE_NAME] is closed."

### Step 8 — Final commit (if in git repository)

**Auto-commit all implementation changes** (if in git repository):
- Check if the working directory is inside a git repository
- Check if git is properly configured (user.name and user.email set)
- If both checks pass, stage and commit all changes with message:
  ```
  feat: [FEATURE_NAME] - complete implementation [skip ci]

  Completed via Analysis-Gated Workflow
  See the analysis bundle for full analysis and implementation details
  ```
- If any check fails, notify the Architect:
  > "Implementation complete but not committed. Please commit manually or enable git."
- This creates the final snapshot of all code changes ready for review, PR, or handoff
  to another agent/skill for validation.

---

## What This Skill Does NOT Do

- Does not write or modify any code
- Does not run without the Architect having accepted the implementation
- Does not close the workflow before the Implementation Summary is complete
- Does not self-initiate — it is entered only on the Architect's acceptance signal

---

## Quality Criteria

Documentation & Cleanup Phase is complete when:

- [ ] All checklist items are ticked or marked skipped with reason
- [ ] Implementation Summary is complete (files created, modified, deleted)
- [ ] All deviations are linked with their rationale
- [ ] Learnings & pitfalls are documented
- [ ] Before/after comparison is present (or explicitly pending measurement)
- [ ] `{{SUMMARY_FILE}}` (the bundle front-door summary) is written
- [ ] `STATE.md` is `COMPLETE`
