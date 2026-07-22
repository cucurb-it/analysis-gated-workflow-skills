---
name: agw-compliance-review
description: the Compliance & Review Phase of the Analysis-Gated Workflow. Use when iterating analysis based on Software Architect ADR annotations. Ensures full compliance before advancing to implementation planning.
---

# Analysis-Gated Workflow — Compliance & Review Skill
## COMPLIANCE & REVIEW PHASE

> File paths, frontmatter, anchors, and the navigation bar in this skill follow the
> **Bundle & File Naming Conventions** defined in `skills/00-governing-workflow/SKILL.md`.
> Every `phase.md` and `summary.md` carries the navigation bar directly under its H1 title;
> `doc-*.md` and ADR files do not. For this phase,
> `{{PHASE_DIR}}` = `{{FOLDER_NAME}}/phase-03-compliance-review`, so
> `{{PHASE_FILE}}` = `{{PHASE_DIR}}/phase.md` and `{{SUMMARY_FILE}}` = `{{PHASE_DIR}}/summary.md`.
> ADRs live as one file each under `{{FOLDER_NAME}}/adrs/` (see the ADR Protocol in the governing skill).

---

## Purpose

This skill governs the Compliance & Review Phase of the Analysis-Gated Workflow. It is an **iterative phase**
that runs as many times as the Architect requires. Its purpose is to ensure the analysis
fully reflects the Architect's intent — incorporating ADR annotations, resolving open
questions, and deepening domain-specific understanding until the Architect is satisfied
that the analysis is ready to drive an Implementation Plan.

Phase 03 is the gate that protects Phase 04 and Phase 05 from being entered prematurely.

---

## Inputs

- The completed Phase 01 and Phase 02 output
- ADR files under `{{FOLDER_NAME}}/adrs/` added by the Architect (may be present at Phase 03 entry, or added during iteration)
- Any additional prompts or clarifications provided by the Architect

---

## COMPLIANCE & REVIEW PHASE Execution

### Step 1 — Log the prompt

Append the Phase 03 initiating prompt (or iteration prompt) to `log.md`, verbatim.

### Step 2 — Read all ADRs

Scan `{{FOLDER_NAME}}/adrs/` and any ADR annotations in phase files. Each ADR file
follows the format defined in the governing skill's ADR Protocol:

```markdown
---
type: ADR
status: DECIDED
date: YYYY-MM-DD
description: <one-line summary>
---

# ADR YYYY-MM-DD — [Subject]

<a id="decision"></a>

|     | Info |
| --- | --- |
| **Subject** | [topic] |
| **Status** | DECIDED / PENDING |
| **Context** | [background and considerations] |
| **Decision** | [what was decided] |
```

For each ADR without a corresponding Compliance response (appended in the same ADR file):
- Record it in your working understanding as an **authoritative constraint**
- Never dispute, reframe, or work around an ADR
- Treat it as a permanent correction to the analysis

### Step 3 — Assess compliance gaps

For each unresolved ADR, identify:
- Which section of the analysis (which `phase.md`) it affects
- What needs to change in that section to comply
- Whether the ADR introduces new analysis requirements not yet covered

For open questions carried forward from Phase 01 or Phase 02:
- Has the Architect's input resolved any of them?
- If yes, document the resolution
- If no, escalate them explicitly before proceeding

### Step 4 — Iterate the analysis

Update the affected phase files to reflect full compliance with each ADR. Record what
changed in `{{PHASE_FILE}}` under `<a id="compliance"></a> ## Compliance Iterations`,
and append the compliance response **inside the ADR's own file**, below its decision:

```markdown
## Compliance YYYY-MM-DD

|     | Info |
| --- | --- |
| **Analysis Updates** | [How the analysis was updated in response] |
| **Sections Affected** | [Which files/sections were changed] |
```

The ADR decision itself is never modified. The compliance response documents what changed
and why, so the reasoning chain is fully traceable — and because it lives in the ADR file,
it accumulates across iterations without re-copying the decision.

### Step 5 — Deepen domain-specific understanding

Beyond ADR compliance, Phase 03 is the opportunity to deepen analysis in areas
that Phase 02 may not have fully explored:

- Domain-specific constraints not visible from the code alone
- Business rules that shape the expected behaviour
- Edge cases identified through the Architect's annotations
- Cross-cutting concerns (security, resilience, observability) if raised by the Architect
- Constraints imposed by the technology stack, framework, or third-party libraries

Only go as deep as the Architect's annotations and prompts indicate is needed.
Do not speculate beyond what the evidence and ADRs support.

### Step 6 — Assess readiness for Implementation Planning Phase

After completing the iteration, evaluate against the **Definition of Ready** for Phase 04.

#### Definition of Ready — Gate to Phase 04

The analysis is ready to proceed to Implementation Planning when ALL of the following
criteria are met:

- [ ] Every ADR file has a compliance response appended within it
- [ ] All open questions from Phase 01 and Phase 02 are resolved or explicitly escalated
- [ ] Project Conventions Confirmation exists and is complete (from Phase 02)
- [ ] The domain understanding is sufficient to write a detailed, phase-structured Implementation Plan without further analysis
- [ ] No unresolved ambiguities remain that could cause the Implementation Plan to be based on incorrect assumptions
- [ ] The Architect has explicitly signalled readiness to proceed to Implementation Planning Phase

**Handling unresolved open questions:**

If any open questions remain unresolved when assessing Definition of Ready, the AI must
create them as **placeholder ADR files** under `{{FOLDER_NAME}}/adrs/`. This is the
**only** circumstance where the AI may author an ADR file (normally ADRs are Architect-only).

Format for a placeholder ADR file (`adrs/YYYY-MM-DD-<slug>.md`):

```markdown
---
type: ADR
status: PENDING
date: YYYY-MM-DD
description: <one-line summary of the open question>
---

# ADR YYYY-MM-DD — [Subject]

<a id="decision"></a>

|     | Info |
| --- | --- |
| **Subject** | [Question text / topic requiring decision] |
| **Status** | PENDING ARCHITECT INPUT |
| **Context** | [Summary of the issue, why it matters, and what the options are] |
| **Decision** | [Most likely options, if known, or "TBD based on Architect input"] |

<a id="consequences"></a>

| Consequences |
| --- |
| DO: [accepted options] <br/> NOT: [rejected options] |
```

These placeholder ADRs become **blocking** — the workflow cannot advance to Phase 04
until the Architect replaces the placeholder content with an actual decision (setting
`status: DECIDED`), or deletes the placeholder if it's no longer relevant.

**If ANY criterion is not met**, document what is missing under the `open-items` anchor in
`{{PHASE_FILE}}`, or create placeholder ADRs as appropriate. Do not proceed until the
Definition of Ready is satisfied.

**If ALL criteria are met**, state clearly:
> "Definition of Ready satisfied. All criteria met. Ready for Phase 04 upon Architect approval."

Then wait for the Architect's explicit go signal before advancing.

### Step 7 — Write the summary file

Write `{{SUMMARY_FILE}}` (`type: Summary`, `phase: "03 — Compliance & Review"`,
`status: AWAITING ARCHITECT REVIEW`, `phase_file: ./phase.md`, one-line `description`):

```markdown
---
type: Summary
phase: "03 — Compliance & Review"
status: AWAITING ARCHITECT REVIEW
phase_file: ./phase.md
description: <one-line outcome>
---

# Phase 03 Summary — Compliance & Review

[Index](../index.md) · [State](../STATE.md) · [Log](../log.md)

---

**Work completed:** [ADRs addressed this iteration; sections updated]

**Outcome:** [Definition of Ready status] → [full detail](./phase.md#compliance)

**Open items carried forward:** [pending ADRs, or "none"] → [detail](./phase.md#open-items)
```

### Step 8 — Update workflow state

Update `{{FOLDER_NAME}}/STATE.md`:
- `Phase Status` → `AWAITING ARCHITECT REVIEW`
- `Pending Architect Action` → `Review compliance updates. Add further ADR annotations if needed, or signal readiness for Implementation Planning Phase.`

### Step 9 — Notify the Architect

> "Compliance & Review Phase iteration is complete. All current ADRs have compliance responses.
> Please review `phase-03-compliance-review/summary.md`. Add further ADR annotations if
> corrections are needed, or signal when you are ready to proceed to Implementation Planning Phase."

---

## Iteration Loop

Phase 03 repeats as follows:

```
Architect adds ADR (file under adrs/ or annotation)
       ↓
AI reads ADR, updates analysis, appends compliance response in the ADR file
       ↓
AI assesses readiness for Implementation Planning Phase
       ↓
If not ready → notify Architect, wait for next ADR or signal
If ready     → notify Architect, wait for explicit go signal
       ↓
Architect signals: "proceed to Implementation Planning Phase"
       ↓
Governing Workflow advances to Phase 04
```

There is no limit on the number of iterations. The Architect controls the exit.

---

## What Phase 03 Does NOT Do

- Does not propose an implementation approach
- Does not write or modify any code
- Does not overwrite, delete, or modify any ADR decision
- Does not self-declare readiness and advance to Phase 04
- Does not skip ADR compliance because the change seems minor
- Does not add analysis beyond what ADRs and prompts indicate is needed

---

## Quality Criteria

Each Compliance & Review Phase iteration is complete when:

- [ ] Every ADR file has a compliance response appended within it
- [ ] All open questions are resolved or explicitly escalated
- [ ] Affected phase files are updated to reflect ADR constraints
- [ ] Domain-specific understanding is deepened where ADRs indicate it is needed
- [ ] Definition of Ready is explicitly evaluated against all six criteria
- [ ] `{{SUMMARY_FILE}}` is written and links back to the phase file
- [ ] `STATE.md` reflects `AWAITING ARCHITECT REVIEW`
