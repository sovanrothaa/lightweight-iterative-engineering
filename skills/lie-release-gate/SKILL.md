---
name: lie-release-gate
description: >
  LIE capability skill: pick the applicable subset of build/test/lint/
  migration/security checks for a change instead of running everything or
  nothing. Part of the lightweight-iterative-engineering (LIE) workflow —
  invoked by LIE's own routing before declaring Level 1+ work done, not on
  its own. If LIE is not already active for this task, invoke
  lightweight-iterative-engineering first instead of this skill directly.
---

# Release Gate

## Purpose

Pick the verification checks that actually apply to a change, instead of either running every
available check by default or skipping verification entirely.

This is about which checks to run before calling work done. What happens to the branch
afterward (merge / PR / keep as-is) is the `lightweight-iterative-engineering` skill's
Completion step, not this file.

---

## Available Checks

Not a checklist to complete in full every time — a menu to select from:

* build
* relevant tests (see `lie-testing` for whether tests are required at all)
* lint / type-check, if the project has one wired in
* database migration verification, if the change includes a migration
* security review, if `lie-security`'s trigger list applies
* rollback readiness, only for genuinely high-risk or destructive changes
* documentation update, only if the change alters a documented interface or behavior

---

## Selecting the Subset

Match checks to what actually changed:

* **Docs/config-only change** → no build, no test run beyond a sanity check that config parses.
* **Ordinary code change** → build (if applicable) + relevant tests per `lie-testing`.
* **Change with a database migration** → add migration verification (does it apply cleanly,
  does it roll back cleanly if the project supports that).
* **Change on the shared risk list** (`lie-review`'s risk-based escalation) → add
  security review and consider rollback readiness.
* **Major, high-risk, or destructive change** → run the fuller applicable subset; don't skip
  rollback readiness here.

Running a check that doesn't apply to the change (a full security review on a README fix) is
the same mistake as skipping one that does — both waste evidence-gathering budget without
improving confidence.

---

## Principle

> **Run the checks the change actually needs — no more, no fewer.**
