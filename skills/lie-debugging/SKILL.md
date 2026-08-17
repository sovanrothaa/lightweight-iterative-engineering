---
name: lie-debugging
description: >
  LIE capability skill: systematic reproduce-investigate-root cause-fix-verify
  workflow for diagnosing defects, with a 3-failed-fix escalation rule. Part
  of the lightweight-iterative-engineering (LIE) workflow — invoked by LIE's
  own routing whenever the primary task is diagnosing a defect, not on its
  own. If LIE is not already active for this task, invoke
  lightweight-iterative-engineering first instead of this skill directly.
---

# Debugging

## Purpose

Provide a systematic approach for diagnosing and fixing software defects.

Do not guess at fixes when the cause can reasonably be investigated.

---

## Workflow

Use:

```text
Reproduce
   ↓
Investigate
   ↓
Identify Root Cause
   ↓
Fix
   ↓
Verify
```

---

## 1. Reproduce

Establish the failure when practical.

Determine:

* what fails
* when it fails
* expected behavior
* actual behavior
* reproducibility
* relevant inputs or conditions

If the problem cannot currently be reproduced, gather evidence before guessing.

---

## 2. Investigate

Inspect relevant:

* code
* logs
* stack traces
* requests/responses
* configuration
* state
* recent changes
* dependencies

Follow evidence toward the cause.

Avoid broad speculative changes.

If a working, analogous example exists elsewhere in the codebase (same kind of operation succeeding on a different input, or the same pattern implemented correctly in a sibling module), compare against it before theorizing from scratch — differences between the working and broken cases are often the fastest route to the cause.

---

## 3. Identify Root Cause

Before fixing, determine the most likely root cause.

Distinguish:

* symptom
* immediate cause
* underlying cause

State the hypothesis explicitly ("X is the cause because Y") before acting on it — this is what step 4 tests, and vague hypotheses produce vague fixes.

Do not stop at making the visible error disappear if the underlying defect remains.

---

## 4. Fix

Implement the smallest appropriate fix.

Prefer fixing the root cause over adding workarounds.

Avoid unrelated refactoring unless necessary for the fix.

If a fix doesn't resolve the problem, don't immediately try another fix — return to step 2 (Investigate) with what the failed attempt revealed. After 3 failed fix attempts, stop fixing: the pattern is usually that each attempt is treating a symptom of something structurally wrong, not that the next attempt will be the one that works. Question whether the underlying approach or architecture is sound before trying a fourth.

---

## 5. Verify

Verify that:

* the original problem is resolved
* the expected behavior works
* the fix does not introduce an obvious regression

Run `lie-testing`'s cheap-first signal check to determine whether a regression test is warranted — a bug fix commonly satisfies signal 1 or 2 on its own (e.g. it touches a file that already has a test sibling). If no signal is found and the fix is not high-risk, verify via reproduction and manual check instead of writing a test.

---

## Principle

> **Do not guess when evidence can tell you what is wrong.**