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

---

## 3. Identify Root Cause

Before fixing, determine the most likely root cause.

Distinguish:

* symptom
* immediate cause
* underlying cause

Do not stop at making the visible error disappear if the underlying defect remains.

---

## 4. Fix

Implement the smallest appropriate fix.

Prefer fixing the root cause over adding workarounds.

Avoid unrelated refactoring unless necessary for the fix.

---

## 5. Verify

Verify that:

* the original problem is resolved
* the expected behavior works
* the fix does not introduce an obvious regression

Run the testing sub-skill's cheap-first signal check (see `skills/testing`) to determine whether a regression test is warranted — a bug fix commonly satisfies signal 1 or 2 on its own (e.g. it touches a file that already has a test sibling). If no signal is found and the fix is not high-risk, verify via reproduction and manual check instead of writing a test.

---

## Principle

> **Do not guess when evidence can tell you what is wrong.**