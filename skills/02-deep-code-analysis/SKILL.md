---
name: agw-deep-code-analysis
description: Phase 02 of the Analysis-Gated Workflow. Use when performing a deep analysis of an existing codebase — architecture, patterns, performance, coding style, and project conventions. No implementation.
---

# Analysis-Gated Workflow — Deep Code Analysis Skill
## Phase 02

---

## Purpose

This skill governs Phase 02 of the Analysis-Gated Workflow. Its responsibility is to
produce a **deep, evidence-based understanding of the existing implementation** — how it
works, what patterns it uses, what its performance characteristics are, how it relates
to the rest of the codebase, and what the current state truly is before any change is made.

Phase 02 is driven by **reading the code**, not by assumptions.
Every finding must be traceable to something observed in the codebase.
No implementation is proposed in this phase — only understanding is built.

---

## Inputs

- The completed Phase 01 output in the ANALYSIS document
- Access to the codebase via Claude Code CLI or Copilot CLI
- The specifics provided by the Architect: class name, pattern, module, domain, etc.

---

## Phase 02 Execution

### Step 1 — Log the prompt

Append the Phase 02 initiating prompt to the `## Prompt Log` section, verbatim.

### Step 2 — Orient in the codebase

Before diving into specifics, establish orientation:

- Locate the primary class, module, or component under analysis
- Identify its position in the broader architecture or pipeline
- Identify its direct dependencies and dependents
- Note the project or namespace it belongs to

### Step 3 — Deep analysis

Analyse the following dimensions systematically. Each finding goes into the
`## Deep Code Analysis` section of the ANALYSIS document under its own subsection.

#### 3.1 — Architecture Overview

- Position in the system: where does this component sit in the pipeline, layer, or domain?
- Class hierarchy: base classes, interfaces implemented, inheritance chain
- Dependencies: what does it depend on, and what depends on it?
- Lifecycle: how is it instantiated, invoked, and disposed?

#### 3.2 — Behavioural Analysis

- What does this component do, step by step?
- What are its execution paths? Map all conditional branches.
- What are its inputs and outputs?
- What side effects does it produce?
- What state does it read and what state does it mutate?

Document the primary execution flow as a structured outline or ASCII diagram,
matching the style of the example ANALYSIS document.

#### 3.3 — Data Model

- What data structures does this component consume and produce?
- What are the key properties, their types, and their meanings?
- What are typical cardinalities (e.g. "2–20 building levels per project")?

#### 3.4 — Coding Patterns & Style

- What patterns are used? (e.g. template method, strategy, pipeline processor)
- What naming conventions are followed?
- What error handling patterns are present?
- What logging or instrumentation exists?
- Are there any project-specific practices or conventions observable in this component?

**Project conventions scan:**
Before documenting coding patterns, scan the following folders in the repository root
if they exist — they contain authoritative project-specific conventions, AI instructions,
and workflow rules that must be respected throughout the analysis and implementation:

- `.claude/` — Claude Code specific instructions, project rules, and AI behaviour directives
- `.github/` — workflow definitions, contribution guidelines, pull request templates,
  and coding standards

Document any relevant conventions found. These take precedence over general coding
assumptions.

#### 3.5 — Performance Characteristics

- Are there any O(n), O(n²), or worse complexity operations?
- Are there repeated traversals of large collections?
- Are there known-expensive API calls (document manipulation, I/O, etc.)?
- Is there any existing documentation or commentary about performance in the code?
- Provide estimated cost breakdown where evidence supports it.

#### 3.6 — Relationship to Similar Components

- Are there similar components in the codebase that solve the same or related problems?
- Do any of them use better or different patterns?
- Are there shared utilities or helpers used across these components?
- Does the component under analysis deviate from the patterns used by its peers?

#### 3.7 — Template Structure (if applicable)

- If the component operates on a document template, map the relevant template markers,
  section structure, and table references.

#### 3.8 — Known Issues & Code Commentary

- Are there any TODO, FIXME, HACK, or NOTE comments in the code?
- Are there any inline comments that document known limitations or workarounds?
- Quote these verbatim — they are evidence.

### Step 4 — Populate Executive Summary

Once the deep analysis is complete, write the `## Executive Summary` section:

- The single most important finding (the dominant issue or characteristic)
- The root cause, if identifiable
- The key insight that shapes everything else
- Keep it tight — the detail lives in the sections below it

### Step 5 — Update Workflow State

Update the `## Workflow State` block:
- `Phase Status` → `AWAITING ARCHITECT REVIEW`
- `Pending Architect Action` → `Review Phase 02 output. Add ADR annotations if corrections needed. Signal readiness for Phase 03.`

### Step 6 — Notify the Architect

> "Phase 02 is complete. Please review the ANALYSIS document. Add ADR annotations
> where corrections or decisions are needed, then signal when you are ready to
> proceed to Phase 03."

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
- [ ] `.claude/` and `.github/` folders scanned for project conventions
- [ ] Coding patterns and project-specific practices are identified
- [ ] Performance characteristics are analysed with complexity estimates
- [ ] Comparison with similar components is documented
- [ ] Known issues and code commentary are captured verbatim
- [ ] Executive Summary is written
- [ ] Workflow State reflects `AWAITING ARCHITECT REVIEW`
