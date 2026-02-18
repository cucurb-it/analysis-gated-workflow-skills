---
name: agw-compliance-review
description: Phase 03 of the Analysis-Gated Workflow. Use when iterating analysis based on Software Architect ADR annotations. Ensures full compliance before advancing to implementation planning.
---

# Analysis-Gated Workflow — Compliance & Review Skill
## Phase 03

---

## Purpose

This skill governs Phase 03 of the Analysis-Gated Workflow. It is an **iterative phase**
that runs as many times as the Architect requires. Its purpose is to ensure the analysis
fully reflects the Architect's intent — incorporating ADR annotations, resolving open
questions, and deepening domain-specific understanding until the Architect is satisfied
that the analysis is ready to drive an Implementation Plan.

Phase 03 is the gate that protects Phase 04 and Phase 05 from being entered prematurely.

---

## Inputs

- The completed Phase 01 and Phase 02 output in the ANALYSIS document
- ADR annotations added by the Architect (may be present at Phase 03 entry, or added during iteration)
- Any additional prompts or clarifications provided by the Architect

---

## Phase 03 Execution

### Step 1 — Log the prompt

Append the Phase 03 initiating prompt (or iteration prompt) to the `## Prompt Log`
section, verbatim.

### Step 2 — Read all ADRs

Scan the entire ANALYSIS document for ADR annotations:

```
> **ADR [YYYY-MM-DD]:** [text]
```

For each ADR found without a corresponding Compliance response:
- Record it in your working understanding as an **authoritative constraint**
- Never dispute, reframe, or work around an ADR
- Treat it as a permanent correction to the analysis

### Step 3 — Assess compliance gaps

For each unresolved ADR, identify:
- Which section of the analysis it affects
- What needs to change in that section to comply
- Whether the ADR introduces new analysis requirements not yet covered

For open questions carried forward from Phase 01 or Phase 02:
- Has the Architect's input resolved any of them?
- If yes, document the resolution
- If no, escalate them explicitly before proceeding

### Step 4 — Iterate the analysis

Update the affected sections of the ANALYSIS document to reflect full compliance
with each ADR. Then, immediately below each ADR, add the compliance response:

```
> **Compliance [YYYY-MM-DD]:** [How the analysis was updated in response]
```

The ADR itself is never modified. The compliance response documents what changed
and why, so the reasoning chain is fully traceable.

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

### Step 6 — Assess readiness for Phase 04

After completing the iteration, evaluate against the **Definition of Ready** for Phase 04.

#### Definition of Ready — Gate to Phase 04

The analysis is ready to proceed to Implementation Planning when ALL of the following
criteria are met:

- [ ] All ADRs in the document have a compliance response directly below them
- [ ] All open questions from Phase 01 and Phase 02 are resolved or explicitly escalated
- [ ] Project Conventions Confirmation table exists and is complete (from Phase 02)
- [ ] The domain understanding is sufficient to write a detailed, phase-structured Implementation Plan without further analysis
- [ ] No unresolved ambiguities remain that could cause the Implementation Plan to be based on incorrect assumptions
- [ ] The Architect has explicitly signalled readiness to proceed to Phase 04

**If ANY criterion is not met**, document what is missing in `## Open Questions` or
flag it explicitly to the Architect. Do not proceed until the Definition of Ready
is satisfied.

**If ALL criteria are met**, state clearly:
> "Definition of Ready satisfied. All criteria met. Ready for Phase 04 upon Architect approval."

Then wait for the Architect's explicit go signal before advancing.

### Step 7 — Update Workflow State

After each iteration:

```
Current Phase:           PHASE 03
Phase Status:            AWAITING ARCHITECT REVIEW
Last Updated:            YYYY-MM-DD
Pending Architect Action: Review compliance updates.
                          Add further ADR annotations if needed,
                          or signal readiness for Phase 04.
```

### Step 8 — Notify the Architect

> "Phase 03 iteration is complete. All current ADRs have compliance responses.
> Please review the updates. Add further ADR annotations if corrections are needed,
> or signal when you are ready to proceed to Phase 04."

---

## Iteration Loop

Phase 03 repeats as follows:

```
Architect adds ADR annotation
       ↓
AI reads ADR, updates analysis, adds compliance response
       ↓
AI assesses readiness for Phase 04
       ↓
If not ready → notify Architect, wait for next annotation or signal
If ready     → notify Architect, wait for explicit go signal
       ↓
Architect signals: "proceed to Phase 04"
       ↓
Governing Workflow advances to Phase 04
```

There is no limit on the number of iterations. The Architect controls the exit.

---

## What Phase 03 Does NOT Do

- Does not propose an implementation approach
- Does not write or modify any code
- Does not overwrite, delete, or modify any ADR
- Does not self-declare readiness and advance to Phase 04
- Does not skip ADR compliance because the change seems minor
- Does not add analysis beyond what ADRs and prompts indicate is needed

---

## Quality Criteria

Each Phase 03 iteration is complete when:

- [ ] All ADRs in the document have a compliance response directly below them
- [ ] All open questions are resolved or explicitly escalated
- [ ] Affected analysis sections are updated to reflect ADR constraints
- [ ] Domain-specific understanding is deepened where ADRs indicate it is needed
- [ ] Definition of Ready is explicitly evaluated against all six criteria
- [ ] Workflow State reflects `AWAITING ARCHITECT REVIEW`