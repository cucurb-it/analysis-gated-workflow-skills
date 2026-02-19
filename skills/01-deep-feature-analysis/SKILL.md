---
name: agw-deep-feature-analysis
description: Phase 01 of the Analysis-Gated Workflow. Use when capturing and structuring a deep understanding of a requested feature or refactoring, based on Architect and user input. No codebase access. No implementation.
---

# Analysis-Gated Workflow — Deep Feature Analysis Skill
## DEEP FEATURE ANALYSIS PHASE

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
- `{{FOLDER_NAME}}` — the working folder for this analysis
- Specifics provided by the user: class name, pattern, business capability,
  service contract, domain name, or similar
- The user's description of the required feature or expected refactoring

If any of these inputs are missing or ambiguous, ask clarifying questions
**before writing anything to the ANALYSIS document**.

---

## Phase 01 Execution

### Step 1 — Log the prompt

Append the initiating prompt to the `## Prompt Log` section of the ANALYSIS document,
verbatim, before doing anything else.

### Step 2 — Analyse the request

Before writing, reason carefully about the following:

- What is being asked — in precise technical terms
- What domain or business capability is involved
- What the scope appears to be — what is included and what is explicitly or implicitly excluded
- What terminology the user is using — preserve it exactly, do not substitute your own
- What ambiguities or gaps exist in the description

### Step 3 — Write to the ANALYSIS document

Populate the `## Feature or Refactoring Description` section with:

#### 3.1 — Request Summary
A precise restatement of what has been requested, in your own words.
This is not a copy of the user's prompt — it is your structured understanding of it.

#### 3.2 — Domain Context
The business or technical domain this feature or refactoring belongs to.
What capability it serves. What problem it solves or what improvement it delivers.

#### 3.3 — Scope
What is in scope. What is explicitly out of scope. What is unclear and requires
clarification before proceeding.

#### 3.4 — Key Concepts & Terminology
The domain-specific terms, class names, patterns, or concepts identified in the
user's description. Defined as the user uses them — not as general programming terms.

#### 3.5 — Open Questions
Any ambiguities, missing information, or assumptions that need Architect confirmation
before Phase 02 can begin. If there are none, state that explicitly.

### Step 4 — Update Workflow State

Update the `## Workflow State` block:
- `Phase Status` → `AWAITING ARCHITECT REVIEW`
- `Pending Architect Action` → `Review Phase 01 output. Signal readiness for Phase 02.`

### Step 5 — Notify the Architect

Inform the Architect that the Deep Feature Analysis Phase is complete. Do not summarise the ANALYSIS document
in chat — direct the Architect to read it. State clearly what action is expected:
> "Deep Feature Analysis Phase is complete. Please review the ANALYSIS document and signal when you are
> ready to proceed to Deep Code Analysis Phase, or add annotations if corrections are needed."

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
- [ ] The Workflow State reflects `AWAITING ARCHITECT REVIEW`