---
name: agw-implementation-review
description: Optional post-06 pass of the Analysis-Gated Workflow. Use when the Architect requests a review of the implemented code against the bundle's plan, project conventions, and security standards. Produces a bundle-native Review Report and, optionally, GitHub Issues. No code is modified.
---

# Analysis-Gated Workflow — Implementation Review Skill
## IMPLEMENTATION REVIEW PHASE (optional, post-06)

> File paths, frontmatter, anchors, and the navigation bar in this skill follow the
> **Bundle & File Naming Conventions** defined in `skills/00-governing-workflow/SKILL.md`.
> For this phase, `{{PHASE_DIR}}` = `{{FOLDER_NAME}}/phase-07-implementation-review`, so
> `{{PHASE_FILE}}` = `{{PHASE_DIR}}/phase.md`, `{{SUMMARY_FILE}}` = `{{PHASE_DIR}}/summary.md`,
> and the Review Report is `{{PHASE_DIR}}/review.md`.

---

## Purpose

This skill governs the **Implementation Review Phase** — an **optional, independent pass**
run **after** the core workflow completes at Phase 06, only when the Architect requests it.
It performs a **state-based review** of the implemented code — it reviews the current
codebase state, not authorship. The code may have been written by AI, human, or both.

It is read-only with respect to the codebase: it reports, it never fixes.
The Architect is the sole decision authority — this skill never auto-approves.

This pass does not gate, and is not gated by, Phase 08 (Implementation Documentation).

---

## Inputs

The review draws on the whole bundle as its blueprint:

- The Implementation Plan in `phase-04-implementation-planning/phase.md`
- The implementation record and deviations in `phase-05-implementation/phase.md`
- All ADRs under `adrs/` — permanent constraints the code must honour
- Project conventions in `.claude/` and `.github/` (as scanned in Phase 02)
- The current codebase, via Claude Code CLI or Copilot CLI
- Git history, to identify the change delta since implementation, where available

---

## IMPLEMENTATION REVIEW PHASE Execution

Initialise `{{PHASE_FILE}}` with frontmatter (`type: Phase`, `phase: "07 — Implementation
Review"`, `status: IN PROGRESS`, one-line `description`) and the navigation bar under the
H1. `phase.md` holds only thin working notes; the deliverable is `review.md`.

### Step 1 — Log the prompt

Append the Architect's review request to `log.md`, verbatim.

### Step 2 — Gather context

- Read the Implementation Plan (`phase-04`), the implementation record and deviations
  (`phase-05`), and all ADRs.
- Confirm the project conventions already captured in `phase-02`'s Project Conventions
  Confirmation; re-scan `.claude/` and `.github/` if the Architect indicates they changed.
- Identify the git change delta since implementation, where git is available.

### Step 3 — Perform the review

Evaluate the current codebase against the checklist below. Every finding must cite
**evidence** — a file and line, or a specific convention rule or ADR it violates.

**Functional requirements**
- Does the implementation satisfy the feature request captured in Phase 01?
- Does it align with the Implementation Plan intent (Phase 04)?

**Project conventions compliance**
- CRITICAL/MANDATORY rules from `.claude/` and `.github/` followed?
- Naming conventions, architecture boundaries, technology constraints respected?
- Critical workflows (migrations, code generation) followed?

**Code quality**
- Simplicity first — is the solution as simple as possible?
- Minimal impact — only necessary code changed?
- No temporary fixes — no TODOs, placeholders, or "fix later" code?
- Senior-developer standards — production-ready?

**Security & safety**
- Input validation present and correct?
- Null safety — all code paths checked for null dereference?
- Error handling appropriate?
- No hardcoded secrets or sensitive data?
- Authentication/authorization checks where needed?

**Deviations & scope**
- Any deviations from the Implementation Plan **not** recorded under
  `phase-05-implementation/phase.md#deviations`? (Recorded, Architect-approved deviations
  are not findings — undocumented ones are.)
- Scope creep (unrelated refactoring, style churn)?
- New dependencies added without approval?

**Change delta (if git available)**
- What changed between the implementation commit and current HEAD?
- Do any post-implementation human changes introduce issues?

Severity scale for every finding: **CRITICAL · HIGH · MEDIUM · LOW · INFO**.

### Step 4 — Write the Review Report

Write `{{PHASE_DIR}}/review.md` with frontmatter (`type: Review`, `phase: "07 —
Implementation Review"`, `status: COMPLETE`, `assessment: <APPROVED | CHANGES REQUESTED |
REJECTED>`, one-line `description`) and the navigation bar under the H1. Body:

```markdown
# [FEATURE_NAME] — Implementation Review

[Index](../index.md) · [State](../STATE.md) · [Log](../log.md)

---

**Date:** YYYY-MM-DD
**Reviewer:** [Architect name — never the AI] (with [AI model] co-review)
**Branch/Commit:** [branch or commit SHA]

## Summary

| Field | Value |
|---|---|
| Overall Assessment | APPROVED / CHANGES REQUESTED / REJECTED |
| Critical | [count] |
| High | [count] |
| Medium | [count] |
| Low | [count] |

**Key findings:** [one or two lines]

## Findings by Severity

### CRITICAL

#### [Finding title]
| | |
|---|---|
| **File** | `path/to/file.cs:line` |
| **Description** | [what the issue is] |
| **Evidence** | [code reference or snippet] |
| **Recommendation** | [how to fix] |
| **Reference** | [ADR, convention rule, or plan section violated] |

### HIGH / MEDIUM / LOW / INFO
[same format; omit a severity with no findings]

## Compliance Assessment

- Project conventions: [followed / violations listed]
- Implementation Plan alignment: [aligned / deviations listed]
- ADR compliance: [each ADR honoured / violations listed]

## Change Delta (if git available)

| File | +/- | Type of change | Issue introduced? |
|---|---|---|---|
| `path` | +25/-10 | refactor | none |

## Recommendations

- **Must Fix (blocking):** [list, or "none"]
- **Should Fix (non-blocking):** [list, or "none"]
- **Consider (optional):** [list, or "none"]

## Approval Status

| | |
|---|---|
| Status | APPROVED / CHANGES REQUESTED / REJECTED |
| Rationale | [why] |
| Next steps | [what should happen next] |
```

The reviewer is always a person; the AI is only a co-reviewer and never the named reviewer
or author.

### Step 5 — Write the summary file

Write `{{SUMMARY_FILE}}` (`type: Summary`, `phase: "07 — Implementation Review"`,
`status: COMPLETE`, `phase_file: ./phase.md`, one-line `description`) with the navigation
bar. It points to the report:

```markdown
---
type: Summary
phase: "07 — Implementation Review"
status: COMPLETE
phase_file: ./phase.md
description: <one-line review outcome>
---

# Phase 07 Summary — Implementation Review

[Index](../index.md) · [State](../STATE.md) · [Log](../log.md)

---

**Assessment:** [APPROVED / CHANGES REQUESTED / REJECTED]

**Findings:** [N critical, N high, N medium, N low] → [full report](./review.md)

**Recommendation:** [one line]
```

### Step 6 — GitHub Issues (optional)

After the report is saved, offer to create GitHub Issues — but only when the assessment is
not an outright REJECT (issues are not filed for code that is being reverted). Only
**Must Fix** and **Should Fix** findings are eligible.

Follow the protocol in full; it is an action with side effects, so the confirmation gate is
mandatory.

1. **Ask** whether to create issues. If no, stop here.
2. **Collect** in one message: the GitHub repository (`owner/repo`); which findings to file
   (default: all Must Fix + Should Fix); any optional common label.
3. **Map** Must Fix → `bug`, Should Fix → `task`; always add `review-finding`.
4. **Generate** one issue per selected finding:
   - Title: `[SEVERITY] <Finding title>`
   - Body: severity, file:line, link to `review.md`, description, evidence, recommendation.
5. **Confirm** — present the full list of issues to be created, with their labels, and
   **wait for an explicit yes**. Create nothing before this confirmation.
6. **Execute** via `gh`:
   - Check `gh auth status`. If authenticated, run `gh issue create --repo "<repo>"
     --title "..." --body "..." --label "bug,review-finding"` per issue, reporting each
     returned URL.
   - If `gh` is unavailable or unauthenticated, output the commands in one fenced block
     plus a collapsible manual-creation fallback, and do not execute anything.

Rules: only findings confirmed in step 2; never execute without the step-5 confirmation;
MEDIUM/LOW/INFO are never filed; if one `gh` call fails, report it and continue with the
rest.

### Step 7 — Update workflow state

Update `{{FOLDER_NAME}}/STATE.md`:
- `Current Phase` → `IMPLEMENTATION REVIEW PHASE`
- `Phase Status` → `COMPLETE`
- `Pending Architect Action` → `Review the Implementation Review Report. Address findings as you see fit.`

Then regenerate `index.md` so the Phase 07 files appear.

### Step 8 — Notify the Architect

> "Implementation Review is complete. The report is at `phase-07-implementation-review/review.md`
> — assessment: [APPROVED / CHANGES REQUESTED / REJECTED], [N] findings. [If issues were
> created: N GitHub Issues filed.] Implementation Documentation is still available as a
> separate optional pass if you want it."

---

## What This Skill Does NOT Do

- Does not run unless the Architect explicitly requests a review
- Does not modify any code — it reports only
- Does not auto-approve or self-decide the assessment (the Architect decides)
- Does not assume AI authorship — it reviews all code equally
- Does not create GitHub Issues without the explicit confirmation gate
- Does not file issues for MEDIUM/LOW/INFO findings, or when the assessment is REJECT
- Does not name the AI as reviewer or author

---

## Quality Criteria

Implementation Review Phase is complete when:

- [ ] Every finding cites evidence (file:line, or a convention/ADR/plan reference)
- [ ] Findings are categorised by severity (CRITICAL → INFO)
- [ ] Compliance against conventions, the plan, and every ADR is assessed
- [ ] Change delta is documented where git is available
- [ ] `review.md` is written with `type: Review` and the `assessment` field set
- [ ] `{{SUMMARY_FILE}}` is written and links to the report
- [ ] GitHub Issues, if requested, were created only after explicit confirmation
- [ ] `STATE.md` reflects the completed review and `index.md` is regenerated
