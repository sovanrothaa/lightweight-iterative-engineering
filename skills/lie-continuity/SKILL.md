---
name: lie-continuity
description: >
  LIE capability skill: maintain and resume state for a task that genuinely
  spans multiple sessions, so decisions and progress survive a context
  reset. This is the multi-session continuity step under LIE — a generic
  plan-execution-with-review-checkpoints skill doesn't get to run instead
  once LIE is active, even if its own description also matches. Part of the
  lightweight-iterative-engineering (LIE) workflow — invoked by LIE's own
  routing only when a task genuinely spans sessions or context is degrading
  enough that losing track would cost more than maintaining state does, not
  on its own and not a default for every Level 2/3 task. If LIE is not
  already active for this task, invoke lightweight-iterative-engineering
  first instead of this skill directly.
---

# Continuity

## Purpose

Survive a context reset on a task that genuinely spans sessions — without re-deriving decisions
already made or losing track of what's done and what's left. This is state for resuming work,
not a project-management artifact.

---

## When to Invoke

Only when a task genuinely spans sessions, or context is degrading enough that losing track of
decisions would cost more than maintaining the file does. Not a default for every Level 2/3
task, and not for Level 0/1 work — most tasks finish inside one session and need nothing here.

---

## The State File (`.lie/state.md`)

```markdown
## Goal
## Current Status
## Decisions
## Constraints
## Changed Files
## Verification
## Remaining Work
```

Store durable decisions, rationale, status, and evidence — never conversation transcripts,
chain-of-thought, or output easily re-derived by re-reading the repo.

---

## Checkpointing

Update the file after each meaningful milestone in Remaining Work — not only right before
clearing context. A file updated only at session-end loses everything since the last update if
the session ends unexpectedly; checkpointing at each milestone limits that loss to one step.

---

## Resuming

Read the file, then inspect current repo state before continuing — don't take the file's word
for it. Spot-check a couple of entries under Changed Files against what's actually in the repo:
state can drift if anything touched the repo between sessions, and continuing from a stale
assumption costs more than the check does.

---

## Relationship to Review

A resumed session's completed work still gets `lie-review` at its normal risk-based depth. This
skill doesn't add a separate review-checkpoint ceremony on top — continuity and review are two
different concerns, and the review escalation `lie-review` already defines is enough.

---

## Principle

> **Checkpoint what would be expensive to lose. Verify a resumed state before trusting it.**
