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

One reviewer pass per task or batch — reuse the risk-based escalation already defined in `../review/SKILL.md`. Do not:

* run a separate spec-compliance pass and a separate quality pass,
* run a fixed number of fix-and-re-review rounds,
* add a second independent reviewer unless the change is on the shared risk list, the first review found substantial issues, or the user asks for it.

When a review finds issues: fix them, re-run relevant verification, and re-check the affected area. Do not restart the whole review unless the fix materially changed the implementation.

---

## Reporting

No ledger. No workspace directory. No "rulings" writeups by default.

A normal todo list — one line per task, done or not-done — is enough for a single session.

If, and only if, the work genuinely spans multiple sessions and would otherwise lose track of what's done, keep a short status note: one line per completed task, not a structured ledger entry.

---

## Final Whole-Change Review

This sub-skill does not redefine the final whole-change review — that's owned by `../review/SKILL.md` and the root `SKILL.md`'s Step 7. This section only covers how to reach that checkpoint cheaply when subagents are involved: dispatch narrowly, review each task once, batch what's mechanical, skip the ledger.

---

## Principle

> **Dispatch only when isolation pays for itself. Use the cheapest model that can do the job. Batch mechanical work. Review once per task, not once per round. Track progress in a todo list, not a ledger.**
