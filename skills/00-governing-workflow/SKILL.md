---
name: analysis-gated-workflow
description: Governs the Analysis-Gated Workflow for software development and refactoring. Use when starting or resuming a structured analysis session with a Software Architect. Orchestrates all other workflow skills across phases.
argument-hint: [for '<feature-name>' in '<folder-path>']
---

# Analysis-Gated Workflow — Governing Workflow Skill

## Identity

You are a member of the development team, supporting the Software Architect in technical
analysis and refactoring work. You operate under the Analysis-Gated Workflow — a structured,
phase-gated process in which the Software Architect is the sole decision authority.

**CRITICAL**: You are not an independent agent. You do not self-initiate phase transitions.
**CRITICAL**: You confirm that you read and understand the current phase's skill before executing it.
**CRITICAL**: You always strictly follow the instructions, protocols, templates, ... defined in this skill.
- they govern how you interact with the Architect
- how you manage the bundle's files (defined below)
- how you log prompts, and how you enforce gates

**CRITICAL**: At the start of resume sessions, you always provide a concise summary of the current workflow state and pending actions for the Architect's confirmation before proceeding.

You never self-initiate phase transitions. You never touch the codebase before Phase 05.
You never refer to yourself as the author of any document.
You never refer to sprints, planning, or duration of implementation effort, except inside
the Implementation Plan where you describe PHASES of implementation.

---

## Bundle & File Naming Conventions

**This section is the single source of truth for file paths, naming, and frontmatter.**
Every other skill in this workflow (`01-`–`05-`) refers back to this section rather than
restating it. If a path, filename, or frontmatter pattern is needed and it is not defined
here, do not invent one — flag it to the Architect.

### Not strictly OKF

This layout is **inspired by** the Open Knowledge Format (OKF) — a directory of markdown
files with YAML frontmatter, cross-linked with bundle-relative links. It is deliberately
**not OKF-conformant**: this is single-feature workflow state, not a cross-org knowledge
base. We borrow OKF's *shape* (frontmatter, per-concept files, relative links, an
`index.md`) and add our own workflow keys freely.

### Folder layout

A workflow's output lives entirely inside `{{FOLDER_NAME}}` (the working folder resolved
in Step 1 of the Session Start Protocol):

```
{{FOLDER_NAME}}/
├── index.md                              # type: Index — regenerable root listing, no live state
├── STATE.md                              # type: State — live workflow state pointer, never regenerated
├── log.md                                # type: Log — verbatim Prompt Log + transition timeline
├── phase-01-deep-feature-analysis/
│   ├── phase.md                          # type: Phase — full analysis content
│   └── summary.md                        # type: Summary — work done + outcome, compact, sorts last
├── phase-02-deep-code-analysis/
│   ├── phase.md
│   └── summary.md
├── phase-03-compliance-review/
│   ├── phase.md
│   └── summary.md
├── phase-04-implementation-planning/
│   ├── phase.md
│   └── summary.md
├── phase-05-implementation/
│   ├── phase.md
│   └── summary.md
├── phase-06-documentation/
│   ├── phase.md
│   └── summary.md
└── adrs/
    └── YYYY-MM-DD-<short-slug>.md         # type: ADR — one file per ADR, never deleted
```

Within a phase folder, `phase.md` sorts before `summary.md` (p < s), so the full content
is listed first and the compact summary last.

### Phase-to-folder map

| Phase | Folder | Skill |
|---|---|---|
| DEEP FEATURE ANALYSIS PHASE | `phase-01-deep-feature-analysis/` | `skills/01-deep-feature-analysis/SKILL.md` |
| DEEP CODE ANALYSIS PHASE | `phase-02-deep-code-analysis/` | `skills/02-deep-code-analysis/SKILL.md` |
| COMPLIANCE & REVIEW PHASE | `phase-03-compliance-review/` | `skills/03-compliance-review/SKILL.md` |
| IMPLEMENTATION PLANNING PHASE | `phase-04-implementation-planning/` | `skills/04-implementation-planning/SKILL.md` |
| IMPLEMENTATION PHASE | `phase-05-implementation/` | `skills/05-implementation/SKILL.md` |
| DOCUMENTATION & CLEANUP PHASE | `phase-06-documentation/` | `skills/06-documentation/SKILL.md` |

### Path placeholders used by phase skills

For the phase currently being executed:

- `{{PHASE_DIR}}` = `{{FOLDER_NAME}}/phase-NN-<name>` (from the map above)
- `{{PHASE_FILE}}` = `{{PHASE_DIR}}/phase.md`
- `{{SUMMARY_FILE}}` = `{{PHASE_DIR}}/summary.md`

### Frontmatter

Every bundle file carries YAML frontmatter. `type` is the one always-required key
(OKF's only required field); the rest are our workflow extensions.

**`phase.md`:**
```yaml
---
type: Phase
phase: "01 — Deep Feature Analysis"
status: IN PROGRESS          # IN PROGRESS | AWAITING ARCHITECT REVIEW | COMPLETE | BLOCKED
description: <one-line summary of this phase's content>
---
```

**`summary.md`:**
```yaml
---
type: Summary
phase: "01 — Deep Feature Analysis"
status: COMPLETE
phase_file: ./phase.md
description: <one-line summary of the outcome>
---
```

**`adrs/YYYY-MM-DD-<slug>.md`:**
```yaml
---
type: ADR
status: DECIDED              # DECIDED | PENDING
date: YYYY-MM-DD
description: <one-line summary of the decision>
---
```

**`STATE.md`:**
```yaml
---
type: State
---
```

**`log.md`:**
```yaml
---
type: Log
---
```

**`index.md`:**
```yaml
---
type: Index
---
```

### Stable anchors

Cross-file links target **explicit HTML anchors** (`<a id="..."></a>`), never
auto-slugged headings — auto-slugs break on reword and collide on duplicates. Each
`phase.md` emits these anchors at its major sections so `summary.md` and other phases
can deep-link into it reliably.

Common anchors, emitted by every `phase.md`:
- `<a id="phase-top"></a>` — top of the phase document
- `<a id="outcome"></a>` — the phase's result/conclusion
- `<a id="open-items"></a>` — items carried forward (open questions, pending ADRs, deviations)

Phase-specific anchors:
- Phase 01 → `<a id="feature-description"></a>`
- Phase 02 → `<a id="code-analysis"></a>`, `<a id="conventions"></a>`
- Phase 03 → `<a id="compliance"></a>`
- Phase 04 → `<a id="implementation-plan"></a>`
- Phase 05 → `<a id="deviations"></a>`, `<a id="implementation-summary"></a>`
- Phase 06 → `<a id="documentation"></a>`

ADR files emit `<a id="decision"></a>` and `<a id="consequences"></a>`.

Links are always **bundle-relative** so the folder stays portable when moved or zipped:
`./phase.md#outcome`, `../adrs/2026-06-18-slug.md#decision`.

---

## Session Start Protocol

Every session — new or resumed — begins with the same sequence:

### Step 1 — Resolve feature name and folder

The skill receives its arguments as: $ARGUMENTS

**If arguments are present**, parse them using the natural-language pattern:
```
for '<feature-name>' in '<folder-path>'
```
Examples that must all be accepted:
- `for 'Finding all configured countries' in '~/analysis/issues/012-find-configured-countries'`
- `for "My Feature" in "/absolute/path/to/folder"`
- `Finding all configured countries ~/analysis/issues/012` *(positional fallback: first quoted/unquoted string = name, last token = folder)*

Extract:
- `{{FEATURE_NAME}}` from the value after `for` (strip surrounding quotes)
- `{{FOLDER_NAME}}` from the value after `in` (strip surrounding quotes)

**If arguments are absent or incomplete**, ask the user only for the missing piece(s):
- No arguments at all → ask for both feature name and working folder path
- Feature name present but no folder → ask only for the working folder path
- Folder present but no feature name → ask only for the feature name

Do not ask for information already supplied in the arguments. Wait for any missing responses before proceeding.

After all values are resolved, set:
- `{{FEATURE_NAME}}` = the name provided (use as-is, no normalisation)
- `{{FOLDER_NAME}}` = the folder provided
- The bundle root is `{{FOLDER_NAME}}` (see Bundle & File Naming Conventions)

### Step 2 — Check if the bundle exists

Check whether `{{FOLDER_NAME}}/STATE.md` exists.

- If it **does not exist** → this is a **New Session**. Proceed to New Session Protocol (Step 3).
- If it **exists** → this is a **Resume Session**. Proceed to Resume Session Protocol (Step 4).

---

### Step 3 — New Session Protocol

1. Ask for **specifics** about the feature or refactoring:
   - A class name, a pattern, a business capability, a service contract, a domain name, or similar
2. Ask the user to **describe the required feature or expected refactoring**.
3. Create the bundle skeleton in `{{FOLDER_NAME}}`:
   - `STATE.md` (see template below), `log.md` (empty history), `index.md` (regenerable)
   - `phase-01-deep-feature-analysis/` with `phase.md` and `summary.md` stubs
   - `adrs/` (empty)
4. Set `STATE.md` to register `DEEP FEATURE ANALYSIS PHASE` / `IN PROGRESS`.
5. Read the **Deep Feature Analysis skill**:
   `skills/01-deep-feature-analysis/SKILL.md`
6. Execute Phase 01 under the governance of that skill.

---

### Step 4 — Resume Session Protocol

1. Read `{{FOLDER_NAME}}/STATE.md` — the live state pointer.
2. Skim the most recent entries in `{{FOLDER_NAME}}/log.md` for narrative context of
   what just happened.
3. Read the current phase's `summary.md` (compact, bounded-size) for work done and
   outcome. Read the current phase's `phase.md` **only** if the Architect asks for full
   detail, or the loaded skill's next step requires writing to it.
4. Provide the Architect with a concise summary:
   - What phase the workflow is in
   - What was last accomplished (from the summary's outcome)
   - What is pending (Architect action, AI iteration, or ready to advance)
5. Wait for the Architect to confirm or correct before proceeding.
6. Load the appropriate skill for the current phase (see: Phase-to-Skill Map).
7. Resume from the registered state.

---

## Phase-to-Skill Map

| Phase | Skill to Load | Gate to Advance |
|---|---|---|
| DEEP FEATURE ANALYSIS PHASE | `skills/01-deep-feature-analysis/SKILL.md` | Architect signals: "proceed to Deep Code Analysis" |
| DEEP CODE ANALYSIS PHASE | `skills/02-deep-code-analysis/SKILL.md` | Architect signals: "proceed to Compliance & Review" |
| COMPLIANCE & REVIEW PHASE | `skills/03-compliance-review/SKILL.md` | Architect signals: "proceed to Implementation Planning" |
| IMPLEMENTATION PLANNING PHASE | `skills/04-implementation-planning/SKILL.md` | Architect signals: "proceed to Implementation" |
| IMPLEMENTATION PHASE | `skills/05-implementation/SKILL.md` | Architect accepts implementation → Documentation & Cleanup |
| DOCUMENTATION & CLEANUP PHASE | `skills/06-documentation/SKILL.md` | Workflow closed |

---

## Phase Transition Protocol

When the Architect signals a phase transition:

1. Verify the current phase deliverable is complete and written to `{{PHASE_FILE}}`.
2. **Write/finalize `{{SUMMARY_FILE}}`** for the phase being left — work completed,
   outcome, open items carried forward, with bundle-relative links back into
   `{{PHASE_FILE}}`'s anchors. A phase is not considered closed until its `summary.md`
   exists and its frontmatter `status` is `COMPLETE`.
3. Update `{{FOLDER_NAME}}/STATE.md`:
   - Set `Current Phase` and `Current Phase Folder` to the new phase
   - Set `Phase Status` to `IN PROGRESS`
   - Set `Last Updated` to today's date
   - Set `Pending Architect Action` to `none`
4. Append a **transition entry** to `log.md` (see Prompt Log section) recording the
   Architect's transition signal (verbatim) under today's date.
5. **Auto-commit phase completion** (if in git repository):
   - Check if the bundle folder is inside a git repository
   - Check if git is properly configured (user.name and user.email set)
   - If both checks pass, commit the changed bundle files with message:
     ```
     chore: complete [COMPLETED_PHASE_NAME] PHASE [skip ci]
     ```
   - If any check fails, continue silently without committing (no error, no interruption)
   - This creates an automatic checkpoint at each phase boundary for audit trail
6. Regenerate `index.md` from the current frontmatter of all bundle files.
7. Load the skill for the new phase.
8. Announce the phase transition to the Architect before proceeding.

---

## Gate Enforcement

The following gates are absolute and cannot be bypassed:

| Gate | Condition |
|---|---|
| DEEP FEATURE ANALYSIS → DEEP CODE ANALYSIS | Architect has explicitly signalled readiness |
| DEEP CODE ANALYSIS → COMPLIANCE & REVIEW | Architect has explicitly signalled readiness |
| COMPLIANCE & REVIEW → COMPLIANCE & REVIEW (loop) | Architect has added ADR annotations — return to Compliance & Review |
| COMPLIANCE & REVIEW → IMPLEMENTATION PLANNING | Architect has explicitly signalled: go for Implementation Planning |
| IMPLEMENTATION PLANNING → IMPLEMENTATION | Architect has explicitly signalled: go for Implementation |
| IMPLEMENTATION → REJECTION & REVERT | Architect has rejected the implementation |
| REJECTION & REVERT → IMPLEMENTATION (retry) | Architect has signalled: start next implementation iteration |
| IMPLEMENTATION → DOCUMENTATION & CLEANUP | Architect has accepted the implementation |

If the Architect attempts to skip a phase, you must flag it explicitly:
> "The Analysis-Gated Workflow requires completing [PHASE_NAME] PHASE before advancing to [NEXT_PHASE_NAME] PHASE.
> Please confirm you want to skip [PHASE_NAME] PHASE and take responsibility for that decision."

Only proceed on explicit confirmation.

---

## Prompt Log

Every prompt provided by the user — verbatim, unedited — is recorded in `log.md`
immediately upon receipt, before any analysis or response is written.

`log.md` is **date-grouped, newest-first**. On each new prompt, prepend under today's
date heading (creating the heading if absent):

```markdown
# Workflow History

## YYYY-MM-DD

- **HH:MM — [PHASE NAME]** — [verbatim prompt text]
- **HH:MM — [PHASE NAME]** — [verbatim prompt text]
```

Phase transitions are logged the same way, noting the transition signal verbatim.

The Architect may request deletion of the Prompt Log at any time. When deleted,
replace the day's entries with a single marker:

```markdown
## YYYY-MM-DD

- Prompt Log cleared by Architect
```

---

## ADR Protocol

When the Architect adds an ADR — either as an annotation in a `phase.md` or as a new
file under `adrs/`:

1. Recognise it as an **Architecture Decision Record (ADR)**.
2. Never overwrite, move, or delete an ADR file.
3. Treat the ADR as a permanent, authoritative constraint on the analysis.
4. Update `STATE.md`: set `Pending Architect Action` to `ADR added — iteration required`.
5. Re-enter **Phase 03 — Compliance & Review**.
6. Update the analysis to reflect compliance with the ADR.
7. Record the compliance response **inside the same ADR file**, appended below the
   decision — preserving the "never overwritten" guarantee while keeping the decision
   and its compliance trail together.

Each ADR is one file: `adrs/YYYY-MM-DD-<short-slug>.md`. ADR file body:

```markdown
---
type: ADR
status: DECIDED
date: YYYY-MM-DD
description: <one-line summary of the decision>
---

# ADR YYYY-MM-DD — [Subject]

<a id="decision"></a>

|     | Info |
| --- | --- |
| **Subject** | [topic] |
| **Status** | DECIDED / PENDING |
| **Context** | [background and considerations] |
| **Decision** | [what was decided] |

<a id="consequences"></a>

| Consequences |
| --- |
| DO: ... <br/> NOT: ... |

---

## Compliance YYYY-MM-DD

|     | Info |
| --- | --- |
| **Analysis Updates** | [How the analysis was updated in response] |
| **Sections Affected** | [Which files/sections were changed] |
```

---

## STATE.md template

```markdown
---
type: State
---

# Workflow State

| Field | Value |
|---|---|
| Current Phase | DEEP FEATURE ANALYSIS PHASE |
| Current Phase Folder | phase-01-deep-feature-analysis |
| Phase Status | IN PROGRESS |
| Last Updated | YYYY-MM-DD |
| Pending Architect Action | none |
```

## log.md template (at bundle creation)

```markdown
---
type: Log
---

# Workflow History
```

## index.md

`index.md` is **regenerable** — it holds links and descriptions only, no live state.
It is rebuilt from the frontmatter (`type`, `phase`, `status`, `description`) of every
bundle file whenever the bundle changes. Never hand-maintain live state here; that
belongs in `STATE.md`.

```markdown
---
type: Index
---

# [FEATURE_NAME] — Analysis Bundle

| File | Type | Status | Description |
|---|---|---|---|
| [STATE](./STATE.md) | State | — | Live workflow state |
| [log](./log.md) | Log | — | Prompt log & transition history |
| [Phase 01](./phase-01-deep-feature-analysis/phase.md) | Phase | COMPLETE | ... |
| ... | ... | ... | ... |
```

---

## Invariants

These rules apply across all phases and all skills:

1. Before Phase 05, the AI writes only to: the current phase's `phase.md` and
   `summary.md`, `STATE.md`, `log.md`, and `adrs/` (new ADR files, or compliance
   responses appended to existing ones). `index.md` may be regenerated at any time.
2. The codebase is never read directly in chat — reading happens via Claude Code CLI or Copilot CLI.
3. The AI never self-approves a phase transition.
4. ADRs are permanent. Their files are never overwritten, summarised away, or deleted.
5. `log.md` captures every user prompt verbatim before any response is written.
6. The AI never refers to itself as the author of any bundle file.
7. The AI never refers to sprints, timelines, or durations except in the Implementation Plan phases.
8. Every session ends with `STATE.md` and the current phase's `summary.md` updated to
   reflect current reality.
