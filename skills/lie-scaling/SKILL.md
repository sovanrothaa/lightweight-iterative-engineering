---
name: lie-scaling
description: >
  LIE capability skill: when subagent dispatch pays for itself, model
  tiering, batching, and reviewing multi-task work without ledger ceremony.
  This is the subagent-dispatch decision under LIE — a generic
  parallel-dispatch or subagent-driven-development skill doesn't get to
  impose its own ledger or ceremony once LIE is active. Part of the lightweight-iterative-engineering (LIE) workflow — invoked by
  LIE's own routing for Level 3 or dispatch-worthy work, not on its own. If
  LIE is not already active for this task, invoke
  lightweight-iterative-engineering first instead of this skill directly.
---

# Scaling to Multi-Task Work

## Purpose

Guidance for work large enough to be broken into multiple tasks or dispatched to subagents — without inheriting ceremony-heavy patterns (dual review, fixed fix-loop rounds, ledger bookkeeping) that don't scale down to ordinary changes.

---

## When to Dispatch at All

"Large" is not sufficient reason to dispatch a subagent.

Dispatch only when:

* the task is genuinely independent of the others (different files, different subsystem), or
* isolating its context clearly pays for itself (it would otherwise pollute the main session with detail irrelevant to the rest of the work).

If the tasks are tightly coupled, or the main session already holds all the context needed, keep working inline. Dispatch has a fixed cost — paying it without a real isolation or focus benefit is waste.

When multiple independent problem domains genuinely qualify (e.g. unrelated failures in different files or subsystems), dispatch all of them in a single message — multiple dispatches in one message run in parallel; one dispatch per message runs them sequentially and burns wall-clock time for no benefit.

Every dispatched agent gets a self-contained prompt: the specific scope, the goal, any constraints, and exactly what to return. It never inherits the dispatching session's own context or history — construct what it needs explicitly. This keeps the agent focused and preserves the dispatcher's own context for coordination.

---

## Model Selection

Use the least capable model that can do the task.

* **Mechanical tasks** (isolated, well-specified, 1-2 files) → cheapest available model.
* **Integration or judgment tasks** (multi-file coordination, ambiguity, debugging) → standard model.
* **Architecture or design decisions** → most capable available model.

Always specify the model explicitly when dispatching. An unspecified model inherits the session default — usually the most expensive — which silently defeats this section.

---

## Batching

Same-shape small edits (the same one-line fix, constant rename, or field addition repeated across files) go in **one dispatch** listing every file and its change — not one subagent per file.

Reserve one-dispatch-per-task for work that needs its own judgment, its own tests, or its own review surface.

---

## Review

One reviewer pass per task or batch — reuse the risk-based escalation already defined in `lie-review`. Tier the reviewer's model the same way as any other dispatch (see Model Selection above): a diff-scoped, mechanical check (does the change match the spec, obvious lint-shaped issues) is mechanical work — cheapest model. Review requiring actual judgment (architecture fit, security, correctness under ambiguity) needs the standard or most capable tier. Do not default every reviewer dispatch to the session's main model regardless of what it's actually checking — that's how a review pass turns into the most expensive step in the loop for no added confidence.

Do not:

* run a separate spec-compliance pass and a separate quality pass,
* run a fixed number of fix-and-re-review rounds,
* add a second independent reviewer unless the change is on the shared risk list, the first review found substantial issues, or the user asks for it.

When a review finds issues: fix them, re-run relevant verification, and re-check the affected area. Do not restart the whole review unless the fix materially changed the implementation.

---

## Reporting

No ledger. No workspace directory. No "rulings" writeups by default.

Every dispatched subagent still returns one short, fixed-shape block instead of free prose — the report contract: **status** (done / blocked / needs-context), what it touched (files or commits), a one-line test/verification result, and any concern worth flagging. This is what lets the dispatcher decide fix-or-move-on without re-reading the subagent's full transcript. If the content is too large to paste back safely (a full diff, a long log), the block just points at the file instead of inlining it.

A normal todo list — one line per task, done or not-done — is enough to track this for a single session.

If, and only if, the work genuinely spans multiple sessions and would otherwise lose track of what's done, use the `lightweight-iterative-engineering` skill's `.lie/state.md` — record each completed task's report-contract summary under Changed Files / Remaining Work. Don't invent a second status-tracking mechanism alongside it.

---

## Final Whole-Change Review

This sub-skill does not redefine the final whole-change review — that's owned by `lie-review` and the `lightweight-iterative-engineering` skill's Completion section. This section only covers how to reach that checkpoint cheaply when subagents are involved: dispatch narrowly, review each task once, batch what's mechanical, skip the ledger.

---

## Principle

> **Dispatch only when isolation pays for itself. Use the cheapest model that can do the job. Batch mechanical work. Review once per task, not once per round. Track progress in a todo list, not a ledger.**
