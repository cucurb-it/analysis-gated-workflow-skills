---
name: agw-documentation
description: the Documentation & Cleanup Phase of the Analysis-Gated Workflow. Use after the Architect accepts an implementation, to produce three audience-targeted documents (executive, business, technical) as expansion seeds, plus deviations, learnings, and results. No further code changes.
---

# Analysis-Gated Workflow — Documentation & Cleanup Skill
## DOCUMENTATION & CLEANUP PHASE

> File paths, frontmatter, anchors, and the navigation bar in this skill follow the
> **Bundle & File Naming Conventions** defined in `skills/00-governing-workflow/SKILL.md`.
> Every `phase.md` and `summary.md` carries the navigation bar directly under its H1 title;
> `doc-*.md` and ADR files do not. For this phase,
> `{{PHASE_DIR}}` = `{{FOLDER_NAME}}/phase-06-documentation`, so
> `{{PHASE_FILE}}` = `{{PHASE_DIR}}/phase.md` and `{{SUMMARY_FILE}}` = `{{PHASE_DIR}}/summary.md`.

---

## Purpose

This skill governs the Documentation & Cleanup Phase of the Analysis-Gated Workflow. It is
entered **only after the Architect accepts the implementation** produced in Phase 05.

Its responsibility is to close out the feature by producing **three audience-targeted
documents**, one for each of the typical audiences of a software project:

| File | Audience | Substance |
|---|---|---|
| `doc-executive.md` | Executive / sponsor | Business-outcome brief — capability delivered, risk delta, what it unblocks. **TCO is out of scope.** |
| `doc-business.md` | Business stakeholder | The end-to-end business process the feature enables; UI/UX described as *where that process surfaces*. |
| `doc-technical.md` | Development team | The technical record — files changed, deviations, learnings, measured results. |

All three are produced for **every** feature (uniformity), however briefly. They are
**expansion seeds**, not finished prose:

- They are **never shared verbatim** outside the development team. Every external reader
  receives a redacted and/or expanded version, generated from the seed in a later pass.
- Because of that, each seed is written as **terse, scannable bullets under fixed
  headings** — clean input for an LLM expansion/redaction pass — not polished narrative.
- Each carries `expandable: true` in frontmatter to mark it as seed material.

A feature with little to say to a given audience still gets that audience's file — as a
one-line-per-heading stub. An empty-looking executive brief is a valid, expandable seed;
a missing file is not.

No code is written or modified in this phase.

---

## Inputs

- The accepted implementation recorded in `phase-05-implementation/phase.md`
- The Implementation Plan in `phase-04-implementation-planning/phase.md`
- All deviations recorded under `phase-05-implementation/phase.md#deviations`
- The Deep Feature Analysis (`phase-01-deep-feature-analysis/phase.md`) — the source of the
  **business process and purpose** that `doc-executive.md` and `doc-business.md` draw on
- Access to the codebase (read-only) for confirming what changed

---

## DOCUMENTATION & CLEANUP PHASE Execution

The **deliverables** of this phase are the three audience documents (`doc-executive.md`,
`doc-business.md`, `doc-technical.md`). `{{PHASE_FILE}}` (`phase.md`) holds only thin
working notes for this phase — initialise it with frontmatter (`type: Phase`,
`phase: "06 — Documentation"`, `status: IN PROGRESS`, one-line `description`) and emit the
common anchors (`phase-top`, `outcome`, `open-items`) plus the phase-specific anchor
`documentation`. The substantive content lives in the three `doc-*.md` files, not here.

### Step 1 — Log the prompt

Append the Phase 06 initiating prompt to `log.md`, verbatim.

### Step 2 — Mark all completed checklist items

Verify every checklist item in the Implementation Plan is correctly ticked.
Any item that was not executed must be explicitly noted as `[SKIPPED — reason]`.

### Step 3 — Generate the technical document (`doc-technical.md`)

Write `{{PHASE_DIR}}/doc-technical.md` — the development team's record. Frontmatter:

```yaml
---
type: Doc
audience: technical
expandable: true
status: COMPLETE
description: <one-line technical summary>
---
```

Body — terse bullets under these fixed headings (this is seed material, not prose):

```markdown
# [FEATURE_NAME] — Technical Documentation (seed)

## Files Created
| File | Description |
|---|---|
| `path/to/file` | What it contains and why it was created |

## Files Modified
| File | Changes |
|---|---|
| `path/to/file` | What was changed and why |

## Files Deleted
| File | Reason |
|---|---|
| `path/to/file` | Why it was removed |

## Deviations from Plan
- Link to each deviation in `../phase-05-implementation/phase.md#deviations` with its
  approved rationale. If none: "None."

## Learnings & Pitfalls (this feature)
- [Insight relevant to future work — patterns, constraints, pitfalls]

## Results (if measured)
| Metric | Before | After | Improvement |
|---|---|---|---|
| [Metric] | [value] | [value] | [delta] |

- Leave as `[pending developer measurement]` if not yet available; must be filled before
  the bundle is closed.
```

### Step 4 — Generate the business document (`doc-business.md`)

Write `{{PHASE_DIR}}/doc-business.md` — for non-technical business stakeholders. The
substance is the **business process** the feature enables; UI/UX appears only as *where
that process surfaces*. Draw the process from Phase 01 (Deep Feature Analysis), not from
the code. Frontmatter:

```yaml
---
type: Doc
audience: business
expandable: true
status: COMPLETE
description: <one-line capability summary>
---
```

Body — terse bullets under these fixed headings:

```markdown
# [FEATURE_NAME] — Business Process & Experience (seed)

## Capability
- What can now be done that could not before — in business terms.

## Business Process (end to end)
- Step-by-step flow of the process this feature enables, from trigger to outcome.
- Include human-in-the-loop or out-of-band steps where they exist.

## Where It Surfaces (UI/UX)
- The surface(s) through which the process is exercised (screen, page action, email,
  API, etc.). Populate only what exists — if there is no user-facing surface, say so and
  describe the non-UI touchpoints (e.g. notification emails).

## What Changes for the User
- Concrete before/after from the user's or stakeholder's point of view.

## Not Included
- Explicit boundaries — what this feature deliberately does not do (e.g. deferred to a
  later feature).
```

### Step 5 — Generate the executive document (`doc-executive.md`)

Write `{{PHASE_DIR}}/doc-executive.md` — for executive sponsors. A short, honest
**business-outcome brief** at feature grain. **TCO is explicitly out of scope** — do not
produce cost, licensing, or budget figures; the bundle does not contain those inputs and
fabricating them is a defect. Frontmatter:

```yaml
---
type: Doc
audience: executive
expandable: true
status: COMPLETE
description: <one-line outcome>
---
```

Body — terse bullets under these fixed headings:

```markdown
# [FEATURE_NAME] — Business Outcome Brief (seed)

## Capability Delivered
- The business capability that now exists, in one or two bullets.

## Risk Delta
- Risk closed by this feature: [...]
- Risk opened or accepted (e.g. acknowledged-imperfect model, deferred hardening): [...]

## What It Unblocks
- What this enables next — dependent features, go-live, revenue path, etc.

## Scope Note
- TCO / cost / budget: out of scope for this feature-level brief.
```

Keep every audience document as brief as the feature warrants. Terse is correct — these
are seeds for later expansion, not the documents an external reader will see.

**Reference the Phase 04 intent seeds.** Where `phase-04-implementation-planning/` contains
forward-looking `doc-executive.md` / `doc-business.md`, the Phase 06 (outcome) versions
should reconcile intent against outcome — e.g. "intended X → deviation Z → delivered Y" —
linking back to the Phase 04 seed rather than overwriting it. This makes the
promise-vs-outcome comparison explicit for each audience.

### Step 6 — Write the closing summary file

Write `{{SUMMARY_FILE}}` (`type: Summary`, `phase: "06 — Documentation"`,
`status: COMPLETE`, `phase_file: ./phase.md`, one-line `description`). This is the
bundle's internal front-door — it points to the three audience documents:

```markdown
---
type: Summary
phase: "06 — Documentation"
status: COMPLETE
phase_file: ./phase.md
description: <one-line feature outcome>
---

# Phase 06 Summary — Documentation & Cleanup

[Index](../index.md) · [State](../STATE.md) · [Log](../log.md)

---

**Work completed:** [what was implemented, in one or two lines]

**Outcome:** [result, incl. before/after if measured]

**Audience documents (expansion seeds — not for verbatim external sharing):**
- Executive → [doc-executive.md](./doc-executive.md)
- Business → [doc-business.md](./doc-business.md)
- Technical → [doc-technical.md](./doc-technical.md)

**Deviations:** [count / "none"] → [detail](../phase-05-implementation/phase.md#deviations)
```

### Step 7 — Final workflow state

Update `{{FOLDER_NAME}}/STATE.md`:
- `Current Phase` → `DOCUMENTATION & CLEANUP PHASE`
- `Phase Status` → `COMPLETE`
- `Pending Architect Action` → `none`

### Step 8 — Close the workflow

> "Documentation & Cleanup phase is complete. The analysis bundle is fully updated. The
> Analysis-Gated Workflow for [FEATURE_NAME] is closed."

### Step 9 — Final commit (if in git repository)

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
- Does not close the workflow before all three audience documents are written
- Does not fabricate TCO, cost, or budget figures in the executive document
- Does not write the audience documents as polished prose — they are terse expansion seeds
- Does not treat any audience document as shareable verbatim outside the development team
- Does not self-initiate — it is entered only on the Architect's acceptance signal

---

## Quality Criteria

Documentation & Cleanup Phase is complete when:

- [ ] All checklist items are ticked or marked skipped with reason
- [ ] `doc-technical.md` is written (files created/modified/deleted, deviations, learnings, results)
- [ ] `doc-business.md` is written (business process end-to-end; UI/UX as where it surfaces)
- [ ] `doc-executive.md` is written (outcome brief; TCO explicitly out of scope, no fabricated figures)
- [ ] All three carry `type: Doc`, the correct `audience`, and `expandable: true`
- [ ] Each document is terse, bulleted seed material under its fixed headings (stubbed if little to say)
- [ ] All deviations are linked with their rationale
- [ ] Before/after results are present in `doc-technical.md` (or explicitly pending measurement)
- [ ] `{{SUMMARY_FILE}}` is written and links to all three audience documents
- [ ] `STATE.md` is `COMPLETE`
