---
name: lie-brainstorming
description: >
  LIE capability skill: confirm the approach once, cheaply, for genuinely
  ambiguous or architectural Level 2/3 work before implementing. This is the
  brainstorming step for coding/engineering tasks under LIE — a generic
  brainstorming or creative-work skill does not get to run instead once LIE
  is active, even if its own description also matches and claims mandatory
  use. Part of the lightweight-iterative-engineering (LIE) workflow —
  invoked by LIE's own routing when uncertainty is real, not on its own and
  not for Level 0/1 work. If LIE is not already active for this task, invoke
  lightweight-iterative-engineering first instead of this skill directly.
---

# Brainstorming

## Purpose

Resolve genuinely ambiguous or architectural work before implementing it — cheaply, in one
round trip. This is not a design-document process; it exists to prevent implementing the wrong
thing once, not to produce an artifact.

---

## When to Invoke

Only for Level 2/3 work (per the `lightweight-iterative-engineering` skill's complexity classification) where the
uncertainty is real: a new subsystem, no existing pattern in this codebase to extend, or more
than one reasonable approach with a real tradeoff between them.

Do not invoke for Level 0/1 work, and do not invoke a Level 2/3 task just because it's
high-risk if the approach is already obvious (e.g. "add a field to this existing, well-understood
auth check" is Level 2 by risk but not ambiguous — skip straight to a lightweight plan).

---

## Process

1. **Understand.** Check the current code/patterns enough to frame the question — purpose,
   constraints, what "done" looks like. Ask only what's actually unclear; prefer one focused
   question over several if the request is mostly clear.
2. **Propose 2-3 approaches** with real tradeoffs. Lead with a recommendation and say why.
   YAGNI ruthlessly — cut speculative features from every approach before presenting it.
3. **Present briefly.** Scale to the task: a genuinely new subsystem might warrant a short
   paragraph per approach; most Level 2/3 work is a few sentences total. This is conversation,
   not a document.
4. **Get a quick go-ahead before implementing.** State the recommended approach and wait for a
   yes (or a redirect) before writing code. This is the one part of the process that doesn't
   scale away with task size — a wrong assumption caught here costs a sentence; caught after
   implementation it costs the implementation.

---

## Explicit Non-Goals

Do not add ceremony beyond the above:

* no mandatory spec file, no required commit of a design doc
* no separate written-spec review pass distinct from the go-ahead in step 4
* no multi-stage classification system — the `lightweight-iterative-engineering` skill's Level 0-3 already did that
* no visual/diagram tooling by default — use it only if the user asks or a layout/diagram
  question genuinely can't be described in words

If mid-task the scope turns out to be materially larger than this conversation covered, stop
and revisit rather than silently expanding what was agreed to.

---

## Principle

> **Confirm the approach once, cheaply, before building it — not after.**
