---
name: lie-documentation
description: >
  LIE capability skill: concrete guidance for updating documentation when a
  change alters a documented interface or behavior — the substance behind
  lie-release-gate's documentation-update check. This is the documentation
  step under LIE — a generic documentation-writing skill doesn't get to run
  instead once LIE is active, even if its own description also matches.
  Part of the lightweight-iterative-engineering (LIE) workflow — invoked by
  LIE's own routing when lie-release-gate's documentation-update check
  applies, not on its own. If LIE is not already active for this task,
  invoke lightweight-iterative-engineering first instead of this skill
  directly.
---

# Documentation

## Purpose

Give `lie-release-gate`'s "documentation update" check actual substance — what counts as done,
not just that it's sometimes required.

---

## Trigger

The change alters a documented interface or behavior — a README claim, a public API, a
configuration surface, a documented CLI flag, or similar. Ordinary internal-only changes with no
documented surface don't need this.

---

## What to Update

Identify what's actually documented for the changed surface — README, API reference, docstrings
written as documentation, CHANGELOG, structure trees — and update only what's actually stale.
This is not a full documentation pass triggered by any change; it's fixing what the change made
wrong.

---

## Style

Match the existing documentation's voice, format, and structure. Don't introduce a new
documentation style, section convention, or standalone file unless the user asks or the changed
surface genuinely has no existing home to update.

---

## Proportionality

A one-line behavior change needs a one-line doc fix. A new feature or interface needs fuller
treatment — see this repo's own `lie-evolution`'s "Close It Out" section for a worked example:
update the structure tree, add a changelog line, keep it scoped to what actually changed.

---

## Don't Let Docs Go Stale Silently

If a change makes an existing documented claim false, updating it is required — not optional,
and not conditional on the user asking. A wrong doc actively misleads; no doc at least doesn't
lie.

---

## Don't Over-Document

Same principle as code comments: document the non-obvious, not what the interface already makes
clear. Don't write documentation for undocumented internals "for completeness" — that's scope
the change didn't ask for.

---

## Principle

> **Update what the change made stale. Match existing style. Don't document what's already obvious.**
