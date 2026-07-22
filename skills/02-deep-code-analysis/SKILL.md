---
name: agw-deep-code-analysis
description: the Deep Code Analysis Phase of the Analysis-Gated Workflow. Use when performing a deep analysis of an existing codebase — architecture, patterns, performance, coding style, and project conventions. No implementation.
---

# Analysis-Gated Workflow — Deep Code Analysis Skill
## DEEP CODE ANALYSIS PHASE

> File paths, frontmatter, anchors, and the navigation bar in this skill follow the
> **Bundle & File Naming Conventions** defined in `skills/00-governing-workflow/SKILL.md`.
> Every `phase.md` and `summary.md` carries the navigation bar directly under its H1 title;
> `doc-*.md` and ADR files do not. For this phase,
> `{{PHASE_DIR}}` = `{{FOLDER_NAME}}/phase-02-deep-code-analysis`, so
> `{{PHASE_FILE}}` = `{{PHASE_DIR}}/phase.md` and `{{SUMMARY_FILE}}` = `{{PHASE_DIR}}/summary.md`.

---

## Purpose

This skill governs the Deep Code Analysis Phase of the Analysis-Gated Workflow. Its responsibility is to
produce a **deep, evidence-based understanding of the existing implementation** — how it
works, what patterns it uses, what its performance characteristics are, how it relates
to the rest of the codebase, and what the current state truly is before any change is made.

Phase 02 is driven by **reading the code**, not by assumptions.
Every finding must be traceable to something observed in the codebase.
No implementation is proposed in this phase — only understanding is built.

---

## Inputs

- The completed Phase 01 output (`phase-01-deep-feature-analysis/phase.md`)
- Access to the codebase via Claude Code CLI or Copilot CLI
- The specifics provided by the Architect: class name, pattern, module, domain, etc.

---

## DEEP CODE ANALYSIS PHASE Execution

### Step 1 — Log the prompt

Append the Phase 02 initiating prompt to `log.md`, verbatim.

### Step 2 — Orient in the codebase

Before diving into specifics, establish orientation:

- Locate the primary class, module, or component under analysis
- Identify its position in the broader architecture or pipeline
- Identify its direct dependencies and dependents
- Note the project or namespace it belongs to

### Step 3 — Deep analysis

Write findings to `{{PHASE_FILE}}` (`type: Phase`, `phase: "02 — Deep Code Analysis"`,
`status: IN PROGRESS`, one-line `description`). Emit the common anchors (`phase-top`,
`outcome`, `open-items`) and the phase-specific anchors (`code-analysis`, `conventions`).
Analyse the following dimensions systematically. Each finding goes under its own
subsection beneath `<a id="code-analysis"></a> ## Deep Code Analysis`.

#### Architecture Overview

- Position in the system: where does this component sit in the pipeline, layer, or domain?
- Class hierarchy: base classes, interfaces implemented, inheritance chain
- Dependencies: what does it depend on, and what depends on it?
- Lifecycle: how is it instantiated, invoked, and disposed?

#### Behavioural Analysis

- What does this component do, step by step?
- What are its execution paths? Map all conditional branches.
- What are its inputs and outputs?
- What side effects does it produce?
- What state does it read and what state does it mutate?

Document the primary execution flow as a structured outline or ASCII diagram.

#### Data Model

- What data structures does this component consume and produce?
- What are the key properties, their types, and their meanings?
- What are typical cardinalities (e.g. "2–20 building levels per project")?

#### Coding Patterns & Style

- What patterns are used? (e.g. template method, strategy, pipeline processor)
- What naming conventions are followed?
- What error handling patterns are present?
- What logging or instrumentation exists?
- Are there any project-specific practices or conventions observable in this component?

**Project conventions scan — MANDATORY:**
Before documenting coding patterns, scan the following folders in the repository root
if they exist. Read **every .md file in great detail** to understand the intricacies
of the project's architectural and development guidelines:

- `.claude/` — Claude Code specific instructions, project rules, AI behaviour directives
  - `CLAUDE.md` — Read deeply, absorb every guideline
  - Any other `.md` files — Read in full
- `.github/` — Workflow definitions, contribution guidelines, pull request templates,
  coding standards, and development instructions
  - `github-instructions.md` or similar — Read thoroughly
  - Any other `.md` files — Read everything, skip nothing

After reading, **create a confirmation table** under `<a id="conventions"></a>
## Project Conventions Confirmation` to prove comprehension. For each file read, extract
guidelines in these categories (where applicable):

```markdown
<a id="conventions"></a>
## Project Conventions Confirmation

#### `.claude/CLAUDE.md`
**Read:** ✓  
**CRITICAL/MANDATORY Rules:** [Any rules marked CRITICAL, MANDATORY, or similar — extract verbatim with emphasis]  
**Architecture Boundaries:** [e.g., which layers can call which, strangler fig pattern rules]  
**Technology Constraints:** [e.g., .NET versions, forbidden language features by project]  
**Naming Conventions:** [e.g., project structure patterns, DI registration patterns]  
**Critical Workflows:** [e.g., database migrations, code generation, build patterns]  
**Hard Constraints:** [e.g., minimize changes to X, never edit Y, always use Z]

#### `.github/github-instructions.md` (or similar)
**Read:** ✓  
**CRITICAL/MANDATORY Rules:** [extract if present]  
**Architecture Boundaries:** [extract if present]  
**Technology Constraints:** [extract if present]  
**Naming Conventions:** [extract if present]  
**Critical Workflows:** [extract if present]  
**Hard Constraints:** [extract if present]

#### `instructions/csharp-instructions.md` (or similar)
**Read:** ✓  
**CRITICAL/MANDATORY Rules:** [extract if present]  
**Architecture Boundaries:** [extract if present]  
**Technology Constraints:** [extract if present]  
**Naming Conventions:** [extract if present]  
**Critical Workflows:** [extract if present]  
**Hard Constraints:** [extract if present]

[Repeat for each .md file found in .claude/ and .github/]
```

**CRITICAL**: If any file contains rules marked with words like CRITICAL, MANDATORY, IMPORTANT,
DO NOT, NEVER, ALWAYS — these MUST be extracted verbatim in the "CRITICAL/MANDATORY Rules"
category. Do not paraphrase these — copy them exactly as written, including emphasis.

Extract **everything relevant** — not just 3-5 items. If a category doesn't apply to a file,
write "N/A" for that category. The table must prove the AI read the file in great detail,
not just skimmed it.

If no convention files exist, state explicitly: "No project-specific convention files found."

Document any relevant conventions found. These take precedence over general coding
assumptions.

#### Performance Characteristics

- Are there any O(n), O(n²), or worse complexity operations?
- Are there repeated traversals of large collections?
- Are there known-expensive API calls (document manipulation, I/O, etc.)?
- Is there any existing documentation or commentary about performance in the code?
- Provide estimated cost breakdown where evidence supports it.

#### Relationship to Similar Components

- Are there similar components in the codebase that solve the same or related problems?
- Do any of them use better or different patterns?
- Are there shared utilities or helpers used across these components?
- Does the component under analysis deviate from the patterns used by its peers?

#### Template Structure (if applicable)

- If the component operates on a document template, map the relevant template markers,
  section structure, and table references.

#### Known Issues & Code Commentary

- Are there any TODO, FIXME, HACK, or NOTE comments in the code?
- Are there any inline comments that document known limitations or workarounds?
- Quote these verbatim — they are evidence.

### Step 4 — Executive Summary & Outcome

Under `<a id="outcome"></a> ## Executive Summary`, write:

- The single most important finding (the dominant issue or characteristic)
- The root cause, if identifiable
- The key insight that shapes everything else
- Keep it tight — the detail lives in the sections above it

Record anything still unresolved under `<a id="open-items"></a> ## Open Items`.

### Step 5 — Write the summary file

Write `{{SUMMARY_FILE}}` (`type: Summary`, `phase: "02 — Deep Code Analysis"`,
`status: AWAITING ARCHITECT REVIEW`, `phase_file: ./phase.md`, one-line `description`).
Because Phase 02 content can grow large, the summary must stay compact — work completed,
the executive-summary outcome, and open items only, each linking back into `{{PHASE_FILE}}`:

```markdown
---
type: Summary
phase: "02 — Deep Code Analysis"
status: AWAITING ARCHITECT REVIEW
phase_file: ./phase.md
description: <one-line outcome>
---

# Phase 02 Summary — Deep Code Analysis

[Index](../index.md) · [State](../STATE.md) · [Log](../log.md)

---

**Work completed:** [dimensions analysed; conventions confirmed]

**Outcome:** [dominant finding, one line] → [full detail](./phase.md#outcome)

**Open items carried forward:** [list, or "none"] → [detail](./phase.md#open-items)
```

### Step 6 — Update workflow state

Update `{{FOLDER_NAME}}/STATE.md`:
- `Phase Status` → `AWAITING ARCHITECT REVIEW`
- `Pending Architect Action` → `Review Phase 02 output. Add ADR annotations if corrections needed. Signal readiness for Phase 03.`

### Step 7 — Notify the Architect

> "Deep Code Analysis Phase is complete. Please review `phase-02-deep-code-analysis/summary.md`
> (and the full `phase.md` if you want detail). Add ADR annotations where corrections or
> decisions are needed, then signal when you are ready to proceed to Compliance & Review Phase."

---

## What Phase 02 Does NOT Do

- Does not propose a solution or refactoring approach
- Does not write or modify any code
- Does not skip dimensions of analysis because they seem unimportant
- Does not make findings without tracing them to observed code
- Does not advance to Phase 03 without an explicit Architect signal

---

## Quality Criteria

Phase 02 output is complete when:

- [ ] Architecture overview is documented with position in pipeline/system
- [ ] Full behavioural analysis with all execution paths is documented
- [ ] Data model is documented with types, properties, and typical cardinalities
- [ ] `.claude/` and `.github/` folders scanned — all .md files read in great detail
- [ ] Project Conventions Confirmation table created with extracted guidelines
- [ ] Coding patterns and project-specific practices are identified
- [ ] Performance characteristics are analysed with complexity estimates
- [ ] Comparison with similar components is documented
- [ ] Known issues and code commentary are captured verbatim
- [ ] Executive Summary is written under the `outcome` anchor
- [ ] `{{SUMMARY_FILE}}` is written and links back to the phase file
- [ ] `STATE.md` reflects `AWAITING ARCHITECT REVIEW`
