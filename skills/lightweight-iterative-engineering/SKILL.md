---
name: lightweight-iterative-engineering
description: >
  A pragmatic, lightweight software engineering workflow for AI coding tasks
  that prioritizes fast iteration, proportional process, and minimal
  speculative work. Use when the user wants lightweight or pragmatic
  engineering, minimal ceremony, conditional testing, no default TDD,
  lightweight review, fast feedback, or a proportional alternative to
  ceremony-heavy development workflows, including cheap subagent dispatch on
  multi-task work instead of dual review and fix-loop ceremony. Routes
  selectively to testing, review, debugging, and scaling based on concrete
  signals and task risk.
---

# Lightweight Iterative Engineering

## Purpose

Lightweight Iterative Engineering (LIE) is a lightweight AI-assisted software development workflow focused on:

* fast iteration
* useful feedback
* minimal speculative work
* proportional verification
* efficient use of time and context

LIE favors practical engineering over rigid process.

The goal is not to follow every engineering practice on every task.

The goal is to use the **minimum process necessary to produce correct, maintainable software**.

---

## Activation

Use this workflow when the user explicitly asks to use LIE, Lightweight
Iterative Engineering, lightweight engineering, or LIE mode.

### Explicit LIE Signals

- "use LIE"
- "LIE mode"
- "use Lightweight Iterative Engineering"
- "lightweight engineering"
- "lightweight iterative engineering"
- "use lightweight"
- "LIE workflow"
- "pragmatic workflow"

### Contextual LIE Signals

The following may indicate LIE when the surrounding request clearly indicates
a lightweight engineering workflow:

- "skip TDD"
- "minimal process"
- "conditional testing"
- "don't over-engineer"
- "keep the review lightweight"
- "avoid unnecessary ceremony"

Contextual signals should not activate LIE in isolation when the user's intent
is ambiguous. Prefer the surrounding request and the user's explicit workflow
choice.

When LIE is explicitly selected, use LIE as the primary development workflow
for the current task.

If another general-purpose development workflow is also installed, including
Superpowers, do not automatically apply its conflicting workflow rules.

Other skills may still be used as supporting capabilities when useful, but LIE
controls the development process.

In particular, when LIE is active, do not automatically inherit:

- mandatory TDD
- mandatory test-first development
- dual-agent review
- repeated reviewer/implementer cycles
- review report artifacts
- mandatory planning or brainstorming ceremonies

unless the user explicitly requests them or LIE's routing determines they are
appropriate.

If the user explicitly selects another workflow instead, follow that workflow
rather than LIE.

If no workflow is explicitly selected, do not assume that LIE is active.

---

# Core Principles

## 1. Understand Before Acting

Before making changes:

* Understand what the user wants.
* Inspect the relevant code and existing patterns.
* Identify important constraints.
* Determine whether the task is well-defined or exploratory.

Do not explore unrelated parts of the repository.

Do not ask unnecessary clarification questions when a reasonable interpretation is obvious.

---

## 2. Build Incrementally

Prefer small, useful increments over large speculative implementations.

When the result can be observed early:

```text
Understand → Plan → Build → Observe → Iterate
```

Get useful feedback before investing heavily in work that may need to be discarded.

Do not prematurely optimize, refactor, abstract, document, test, or review work that is still likely to change.

---

## 3. Do Not Default to TDD

Testing is **not automatically required for every task**.

Only invoke the testing sub-skill when a testing requirement is actually detected — see `testing/SKILL.md` for the detection procedure. Detection is signal-based, not assumption-based, and is ordered cheapest-first so resolving it never costs more than the task justifies.

Do not write large numbers of tests simply because code was implemented.

---

## 4. Review Is Mandatory

Every completed implementation receives a review pass before it is considered done.

However, the default review should be lightweight:

* self-review, or
* one quick reviewer pass

The default review should not involve multiple agents, separate review packages, extensive reports, or repeated review cycles.

Escalate review only when the risk or complexity justifies it.

---

## 5. Review Risk, Not Ceremony

Use deeper review for changes involving:

* authentication or authorization
* security
* cryptography
* payments
* sensitive data
* destructive operations
* infrastructure
* significant architectural changes
* high-impact client-facing functionality
* other changes where failure has substantial consequences

For ordinary changes, keep review fast and focused.

This same risk list is reused elsewhere in LIE (e.g. deciding whether to ask the user about testing) rather than maintaining a second taxonomy.

---

## 6. Final Whole-Change Review

When a request consists of multiple related tasks, perform **one final whole-change review** after all tasks are implemented and basic verification is complete.

This review exists to catch issues that individual task reviews cannot see:

* cross-task inconsistencies
* integration problems
* conflicting changes
* duplicated logic
* architectural drift
* unintended interactions
* incomplete end-to-end behavior

Do not replace this with a heavy review after every task.

---

## 7. Evidence Over Assumption

Do not claim that work is complete because the code appears correct.

Use appropriate evidence:

* run relevant commands
* run the application when practical
* inspect output
* reproduce fixes
* inspect the final diff
* run required project checks

Verification should be proportional to the change.

---

## 8. Resolve Ambiguity Cheaply, Not by Default Questioning

When something is unclear, do not treat "ask the user" as the default resolution.

Prefer, in order:

1. A cheap, local signal check (existing file, existing config, existing pattern).
2. A reasonable default consistent with the rest of the codebase.
3. Asking the user — only when cheap checks fail to resolve it **and** the cost of guessing wrong is high (risk-listed work, or irreversible/destructive actions).

Broad exploration performed just to avoid asking a question is not "cheap" — it can cost more than the question would have. Don't do that either. A clarifying question and a speculative exploration pass are both round-trip costs; pick whichever is actually cheaper, and prefer neither when a fast local check settles it.

---

# Workflow

## Step 1 — Understand

Determine:

* What is being requested?
* What part of the system is affected?
* Is the behavior already clearly defined?
* Are there important unknowns?

Load `debugging/SKILL.md` when the task is primarily about diagnosing or fixing a bug.

---

## Step 2 — Plan

For anything beyond a trivial change, establish a concise implementation approach.

The plan should be proportional to the task.

Do not create extensive planning documents for simple work.

For larger work, break the request into manageable tasks.

If those tasks will be dispatched to subagents (rather than worked inline in this session), load `scaling/SKILL.md` before dispatching — it covers when dispatch is actually worth it, model selection, batching, and review, so multi-task work doesn't default into ceremony-heavy patterns.

If, during implementation, the scope turns out to be materially larger than the original plan assumed, pause and confirm with the user before continuing rather than silently expanding scope.

---

## Step 3 — Implement

Build the requested change using existing project patterns.

Prefer:

* simple solutions
* existing abstractions
* small diffs
* incremental implementation
* minimal new dependencies

Avoid:

* speculative architecture
* unnecessary abstractions
* unrelated refactoring
* premature optimization
* work outside the requested scope

---

## Step 4 — Iterate

When the implementation can be meaningfully observed, inspect the result and adjust as necessary.

Prefer:

```text
Build → Observe → Adjust
```

over attempting to predict every detail before seeing the result.

If the user provides feedback, treat it as authoritative clarification of the intended result.

---

## Step 5 — Testing Decision

Before writing tests, run the cheap-first signal check defined in `testing/SKILL.md` ("Detecting a Testing Requirement").

* **Signal found** (existing test file for the touched module, or a wired-in test command in CI/config) → invoke `testing/SKILL.md`.
* **No signal found, normal risk** → skip testing, proceed without asking.
* **No signal found, high risk** (per the risk list in Principle 5) → ask the user once, then proceed based on the answer.

Do not treat "unclear" as a default reason to stop and ask. Ambiguity here is resolved by the two cheap checks in the testing sub-skill, not by a clarifying question, except in the high-risk case above.

Do not create tests merely to increase coverage.

---

## Step 6 — Lightweight Review

Before considering the implementation complete, invoke:

`review/SKILL.md`

using the appropriate review depth.

Default:

**single lightweight review**

Escalate only when justified by risk or complexity.

---

## Step 7 — Final Whole-Change Review

If the work contains multiple related tasks:

After all tasks are implemented and basic checks are green:

`review/SKILL.md`

Perform one final review across the complete change.

This is a single holistic pass.

Do not repeat the full review separately for every task.

---

## Step 8 — Verify

Before declaring completion:

* run required project checks
* run tests that were required or detected
* verify the requested behavior
* inspect the final diff
* confirm there are no obvious unintended changes

Do not claim verification that was not actually performed.

---

# Routing

LIE uses skills selectively.

The diagram below covers a single task's understand → build → review loop. When
Step 2 (Plan) breaks larger work into multiple tasks that will be dispatched to
subagents, load `scaling/SKILL.md` before dispatching — it sits upstream of this
loop, not inside it.

```text
                    TASK
                      │
                      ▼
                UNDERSTAND
                      │
          ┌───────────┼───────────┐
          │           │           │
       Debug?    Test signal?   Normal
          │           │           │
          ▼           ▼           ▼
      debugging    testing      BUILD
          │           │           │
          └───────────┴─────┬─────┘
                            │
                            ▼
                         REVIEW
                            │
                  ┌─────────┴─────────┐
                  │                   │
             More tasks?          Complete
                  │                   │
                 YES                  ▼
                  │             FINAL REVIEW
                  │             if multi-task
                  │                   │
                  └─────────►         ▼
                               VERIFY
```

### Routing rules

**Debugging**

Invoke when the primary task is diagnosing or fixing an existing defect.

If the bug fix triggers a testing signal (or a regression test is the natural verification for that bug), route to testing after the fix, not instead of it — debugging and testing are not mutually exclusive branches.

**Testing**

Invoke only when the cheap-first signal check in `testing/SKILL.md` finds a signal, or the high-risk exception applies.

**Review**

Invoke for every completed implementation.

Use lightweight review by default.

Use deeper review when risk justifies it.

For multi-task work, perform one additional final whole-change review.

**Scaling**

Invoke when Step 2 decides multiple tasks will be dispatched to subagents rather than worked inline. Covers dispatch criteria, model selection, batching, and per-task review depth — see `scaling/SKILL.md`.

Do not automatically invoke every sub-skill.

---

# Efficiency Rules

The agent should continuously avoid work that does not materially improve the result.

Avoid:

* unnecessary test generation
* redundant review passes
* speculative abstractions
* unrelated refactoring
* excessive repository exploration
* unnecessary documentation
* repeated verification that provides no new information
* large context collection without purpose
* asking a clarifying question when a cheap local check would answer it
* exploring broadly just to avoid asking a question — that isn't actually cheaper
* dispatching a subagent on the session's default model when a cheaper one would do
* one subagent dispatch per task when several same-shape tasks could be batched into one

Prefer:

* focused context
* small changes
* existing project patterns
* fast feedback
* proportional verification
* meaningful evidence
* the cheapest signal that actually resolves the ambiguity

When two approaches provide similar confidence, prefer the one requiring less time and context.

---

# Definition of Done

Work is complete when:

1. The requested behavior has been implemented.
2. The implementation fits the existing project.
3. Required verification has been performed.
4. The mandatory lightweight review has been completed.
5. Tests have been completed when a testing signal was detected or the user confirmed they're needed.
6. A final whole-change review has been completed for multi-task work.
7. No obvious unintended changes remain.

Do not continue polishing indefinitely.

---

# Core Rule

> **Build what is needed. Get useful feedback early. Test when a real signal says to. Review every change, but review proportionally. Resolve ambiguity with the cheapest check that actually answers it. Harden the whole result once — not repeatedly.**