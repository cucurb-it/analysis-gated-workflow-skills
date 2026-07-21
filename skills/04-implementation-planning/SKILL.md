---
name: agw-implementation-planning
description: the Implementation Planning Phase of the Analysis-Gated Workflow. Use when producing a detailed, phase-structured Implementation Plan based on completed analysis, together with forward-looking executive and business stakeholder seeds. No code is written. Requires explicit Architect go signal to advance to implementation.
---

# Analysis-Gated Workflow — Implementation Planning Skill
## IMPLEMENTATION PLANNING PHASE

> File paths, frontmatter, and anchors in this skill follow the **Bundle & File Naming
> Conventions** defined in `skills/00-governing-workflow/SKILL.md`. For this phase,
> `{{PHASE_DIR}}` = `{{FOLDER_NAME}}/phase-04-implementation-planning`, so
> `{{PHASE_FILE}}` = `{{PHASE_DIR}}/phase.md` and `{{SUMMARY_FILE}}` = `{{PHASE_DIR}}/summary.md`.

---

## Purpose

This skill governs the Implementation Planning Phase of the Analysis-Gated Workflow. It
produces a **detailed, phase-structured Implementation Plan** based entirely on the analysis
accumulated in Phases 01–03, together with two **forward-looking stakeholder seeds** — an
executive and a business view of *what the plan will deliver and why*.

The Implementation Plan is the contract between the analysis and the implementation.
It must be precise enough that Phase 05 can be executed without further analysis decisions.

The stakeholder seeds let a business or executive stakeholder review the **intent** before
any code is written, while it is still cheap to change. They mirror the Phase 06 close-out
documents, but are forward-looking (intent) rather than retrospective (outcome); Phase 06's
documents later reference them to compare what was promised against what was delivered.

No code is written in this phase.
No code is modified in this phase.
Only planning artifacts are written — the plan and the two stakeholder seeds.

---

## Inputs

- The completed Phase 01, 02, and 03 output
- All ADR files under `{{FOLDER_NAME}}/adrs/` and their compliance responses
- The resolved scope, domain understanding, and identified constraints

---

## IMPLEMENTATION PLANNING PHASE Execution

**The Implementation Plan is a planning artifact only. No code is written,
no files are modified, and no implementation begins in this phase — regardless
of how the prompt is worded. The plan is complete when the Architect explicitly
signals go for Phase 05.**

### Step 1 — Log the prompt

Append the Phase 04 initiating prompt to `log.md`, verbatim.

### Step 2 — Check for existing Implementation Plan

Before writing anything, check whether `{{PHASE_FILE}}` already contains an
Implementation Plan.

- If it **already exists**: do not overwrite it. Notify the Architect:
  > "An Implementation Plan already exists. Do you want me to extend it, replace it,
  > or leave it as-is?"
  Wait for explicit instruction before proceeding.

- If it **does not exist**: proceed to Step 3.

### Step 3 — Review the full analysis

Before writing the Implementation Plan, re-read the Phase 01–03 phase files and all ADRs.
Confirm that:

- The scope is unambiguous
- All ADRs are reflected in the analysis
- No open questions remain unresolved
- The existing code behaviour is fully understood

If any of the above are not satisfied, do not proceed. Flag the gap to the Architect.

### Step 4 — Design the Implementation Plan structure

The Implementation Plan is structured as **numbered phases** — not as tasks, stories,
or time estimates. Each phase represents a logical unit of implementation that can be
built and validated independently.

For each phase, identify:
- What it delivers
- What it depends on (prior phases)
- What validation is needed before the next phase begins

### Step 5 — Write the Implementation Plan

Write `{{PHASE_FILE}}` (`type: Phase`, `phase: "04 — Implementation Planning"`,
`status: IN PROGRESS`, one-line `description`). Emit the common anchors (`phase-top`,
`outcome`, `open-items`) and the phase-specific anchor (`implementation-plan`).
Populate the body under `<a id="implementation-plan"></a> ## Implementation Plan`:

#### Proposed Changes Summary

A concise overview of what will change:
- Files to be created
- Files to be modified
- Files to be deleted
- New classes, methods, or patterns introduced
- Existing patterns eliminated or replaced

#### Strategy Evaluation (if multiple approaches exist)

If the analysis identified multiple viable approaches:

| Strategy | Impact | Risk | Complexity | Verdict |
|---|---|---|---|---|
| Strategy 1 | ★★★★★ | Low | Low | ✅ Selected |
| Strategy 2 | ★★★☆☆ | Medium | High | ❌ Not selected |

State the selected approach and the rationale.

#### Implementation Phases

For each phase:

```
#### Phase N: [Phase Title] [status emoji]

##### N.1 [Subtask title]
- [ ] [Precise, actionable checklist item]
- [ ] [Precise, actionable checklist item]
- [ ] ...

##### N.2 Build and Validate
- [ ] Build [project name]
- [ ] Verify no compilation errors
- [ ] Verify no new warnings
- [ ] [Any phase-specific validation]
```

**Checklist items must be:**
- Specific enough to be unambiguously done or not done
- At the level of individual methods, properties, or files
- Traceable to the analysis (each item should be explainable by something in Phase 02/03)

**Phase structure typically follows:**
1. Create new shared infrastructure (helpers, base classes, result types)
2. Refactor primary component
3. Refactor secondary components (if applicable)
4. Integration testing & validation
5. Documentation & cleanup

#### Risk Mitigation

For each significant risk identified in the analysis:

| Risk | Impact | Mitigation |
|---|---|---|
| [Risk] | High/Medium/Low | [Concrete mitigation step] |

#### Deviations Protocol

State explicitly:
> "Any deviation from this Implementation Plan during Phase 05 must be:
> 1. Stopped immediately
> 2. Documented in `phase-05-implementation/phase.md` under the `deviations` anchor
> 3. Reviewed by the Architect before implementation continues"

Record the selected approach under the `outcome` anchor, and any residual risks or
assumptions under the `open-items` anchor.

### Step 6 — Generate the stakeholder seed documents

Alongside the Implementation Plan, produce two **forward-looking** audience seeds — the
non-technical views of *what the plan will deliver and why*. The Implementation Plan itself
is the technical view, so only the executive and business audiences are added here.

These are **expansion seeds**, identical in form to the Phase 06 seeds: terse bullets under
fixed headings, `expandable: true`, never shared verbatim outside the development team.
The difference is tense — these describe **intent before the build**, not outcome after it.
They are **frozen** once Phase 04 closes: Phase 05 does not edit them, and Phase 06 writes
its own retrospective seeds that reference these rather than overwriting them.

**`{{PHASE_DIR}}/doc-executive.md`** — executive/sponsor, forward-looking:

```markdown
---
type: Doc
audience: executive
expandable: true
status: AWAITING ARCHITECT REVIEW
description: <one-line intended outcome>
---

# [FEATURE_NAME] — Business Outcome Brief (intent, seed)

## Capability To Be Delivered
- The business capability this plan will create, in one or two bullets.

## Risk Delta (expected)
- Risk this is expected to close: [...]
- Risk being accepted or deferred (e.g. acknowledged-imperfect model, later hardening): [...]

## What It Will Unblock
- What this enables next — dependent features, go-live, revenue path, etc.

## Scope Note
- TCO / cost / budget: out of scope for this feature-level brief.
```

**`{{PHASE_DIR}}/doc-business.md`** — business stakeholder, forward-looking:

```markdown
---
type: Doc
audience: business
expandable: true
status: AWAITING ARCHITECT REVIEW
description: <one-line intended capability>
---

# [FEATURE_NAME] — Business Process & Experience (intent, seed)

## Capability
- What will be possible that is not possible today — in business terms.

## Business Process (end to end, intended)
- Step-by-step flow of the process this feature will enable, from trigger to outcome.
- Include human-in-the-loop or out-of-band steps where they apply.

## Where It Will Surface (UI/UX)
- The surface(s) through which the process will be exercised (screen, page action, email,
  API, etc.). Populate only what is planned — if there is no user-facing surface, say so
  and describe the non-UI touchpoints.

## What Will Change For The User
- Concrete before/after from the user's or stakeholder's point of view.

## Not Included
- Explicit boundaries — what this feature deliberately will not do (e.g. deferred to a
  later feature).
```

Keep both as brief as the feature warrants — stub a heading with one line if there is
little to say. They exist so a business or executive stakeholder can review and sanity-check
the **intent** while it is still cheap to change, before any code is written.

### Step 7 — Write the summary file

Write `{{SUMMARY_FILE}}` (`type: Summary`, `phase: "04 — Implementation Planning"`,
`status: AWAITING ARCHITECT REVIEW`, `phase_file: ./phase.md`, one-line `description`).
The front-door links the plan and both stakeholder seeds:

```markdown
---
type: Summary
phase: "04 — Implementation Planning"
status: AWAITING ARCHITECT REVIEW
phase_file: ./phase.md
description: <one-line outcome>
---

# Phase 04 Summary — Implementation Planning

**Work completed:** [plan structure; number of implementation phases]

**Outcome:** [selected strategy, one line] → [full detail](./phase.md#implementation-plan)

**Stakeholder seeds (intent — expansion seeds, not for verbatim external sharing):**
- Executive → [doc-executive.md](./doc-executive.md)
- Business → [doc-business.md](./doc-business.md)

**Open items carried forward:** [residual risks, or "none"] → [detail](./phase.md#open-items)
```

### Step 8 — Update workflow state

Update `{{FOLDER_NAME}}/STATE.md`:
- `Phase Status` → `AWAITING ARCHITECT REVIEW`
- `Pending Architect Action` → `Review Implementation Plan. Add ADR annotations if corrections needed. Signal go for Phase 05 to begin implementation.`

### Step 9 — Notify the Architect

> "Implementation Planning Phase is complete. The Implementation Plan is ready for review in
> `phase-04-implementation-planning/`, together with forward-looking executive and business
> seeds describing what the plan will deliver. Please review it carefully. Add ADR annotations
> if corrections are needed. When you are satisfied, signal go for Phase 05 to begin implementation."

---

## What Phase 04 Does NOT Do

- Does not write or modify any code
- Does not begin implementation "just to validate the plan"
- Does not use sprint, timeline, or duration language
- Does not make the Implementation Plan vague to preserve flexibility
- Does not fabricate TCO, cost, or budget figures in the executive seed
- Does not write the stakeholder seeds as polished prose — they are terse, forward-looking seeds
- Does not treat the stakeholder seeds as an approval gate — the Architect's go signal alone unlocks Phase 05
- Does not advance to Phase 05 without an explicit Architect go signal

---

## Quality Criteria

Phase 04 output is complete when:

- [ ] Proposed changes summary lists all affected files with their change type
- [ ] Strategy evaluation is present if multiple approaches were identified
- [ ] Implementation phases are numbered and sequenced logically
- [ ] Every checklist item is specific, actionable, and traceable to the analysis
- [ ] Each phase ends with a Build and Validate step
- [ ] Risk mitigation table is populated
- [ ] `doc-executive.md` and `doc-business.md` (forward-looking intent seeds) are written, `expandable: true`
- [ ] Executive seed keeps TCO out of scope (no fabricated figures)
- [ ] Deviations protocol is stated
- [ ] `{{SUMMARY_FILE}}` is written and links to the plan and both stakeholder seeds
- [ ] `STATE.md` reflects `AWAITING ARCHITECT REVIEW`
