---
name: lie-evolution
description: >
  LIE capability skill: capture a real, repeated correction to a lie-*
  skill's guidance, or a documented gap in one, into the skill library
  itself instead of relying on being reminded again next session. This is
  the skill-evolution workflow under LIE — a generic skill-evolution or
  skill-authoring skill doesn't get to run instead once LIE is active, even
  if its own description also matches. Part of the
  lightweight-iterative-engineering (LIE) workflow — invoked by LIE's own
  routing when a correction to LIE's own guidance recurs or a gap in a
  lie-*/SKILL.md surfaces, not on its own. If LIE is not already active for
  this task, invoke lightweight-iterative-engineering first instead of this
  skill directly.
---

# Skill Evolution

## Purpose

LIE's own skill library accumulates real operating knowledge over time instead of staying a
static reference. When a `lie-*/SKILL.md` gives wrong or stale guidance, or a non-obvious
procedure keeps recurring uncodified, the fix belongs in the skill — not in a correction that
has to be repeated next session.

---

## Trigger

Invoke when, during LIE-governed work:

* a `lie-*/SKILL.md`'s guidance was wrong, incomplete, or stale for a real situation just
  encountered
* the user corrected an approach that a `lie-*` skill should already have covered
* the same non-obvious, LIE-relevant procedure has recurred 2-3+ times without being codified
  — "twice is a note, three times is a conversation"

---

## Judge Before Acting

Don't invent a separate judgment framework for this — reuse LIE's own:

* Codifying a fix is itself a scope increase beyond the task at hand; hold it to Escalation
  rule 4's bar ("scope must materially increase beyond what was agreed") before acting on it
  mid-task.
* Stop Rules already says not to improve already-correct code without a requirement — a single
  correction without a real recurrence signal is not yet that requirement.
* When unsure whether it will recur, default to not yet. A second occurrence confirms it; note
  the first in `.lie/state.md` if the task already has one, rather than losing the evidence.

---

## Scope: Patch vs. New Skill

* **Patching** an existing `lie-*/SKILL.md`, or the parent's routing/Escalation/Stop Rules
  prose, for a confirmed recurring gap is in scope for autonomous action — small, reversible,
  git-tracked, and stays inside LIE's existing routing table.
* **Creating** a wholly new `lie-*` capability skill changes the routing table, README
  structure, and the `PreToolUse` guard all at once. Treat that as scope materially increasing
  (Escalation rule 4) and real uncertainty (rule 5) — propose it and wait rather than creating
  it unasked.

---

## How to Update a Skill

* `description` is the trigger — match the narrow, competitor-aware phrasing every `lie-*`
  skill already uses; don't broaden it just to "cover more."
* Procedural, not encyclopedic — steps to follow, not background reading.
* Reference the source of truth instead of duplicating it — point at the parent skill's section
  (e.g. "see Escalation rule 4") rather than restating its content.
* Match sibling shape and length (roughly 60-155 lines; `lie-security` is a compact template).

---

## Close It Out

Applies once a patch or new skill is actually ready to ship:

* Update `README.md`'s Structure tree (and its changelog blurb, for a new skill).
* Bump `plugin.json` and `marketplace.json` together — patch for fixing existing guidance,
  minor for a new skill.
* Otherwise this is a normal shippable change — route it through `lie-release-gate` and LIE's
  own Completion step like any other change; skill evolution doesn't get a separate completion
  process.

---

## When to Stop and Ask

* Creating a new `lie-*` capability skill (see Scope above).
* Anything that would change the Escalation or Stop Rules thresholds themselves, not just a
  capability skill's content under them.
* Which skill should own the fix is itself ambiguous.

---

## Principle

> **Codify the second correction, not the first — and fix the skill, not just the instance.**
