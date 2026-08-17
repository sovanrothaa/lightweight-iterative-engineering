---
name: lie-planning
description: >
  LIE capability skill: write the smallest plan that reduces implementation
  risk, sized to task complexity (Level 0-3). This is the planning step
  under LIE — a generic multi-step-task planning skill doesn't get to run
  instead once LIE is active, even if its own description also matches.
  Part of the
  lightweight-iterative-engineering (LIE) workflow — invoked by LIE's own
  routing, not on its own. If LIE is not already active for this task, invoke
  lightweight-iterative-engineering first instead of this skill directly.
---

# Planning

## Purpose

Reduce implementation mistakes with the smallest plan that does the job. The plan exists to
clarify approach before building, not to become a deliverable in its own right.

---

## By Level

**Level 0** — no plan. Inspect, change, verify.

**Level 1** — 3-5 bullets is normally enough:

```markdown
## Goal
[one sentence]

## Approach
[2-3 sentences]

## Steps
1. ...
2. ...

## Verification
- ...
```

**Level 2/3** — expand only where more detail actually reduces implementation risk: affected
components, sequencing if tasks depend on each other, and what a subagent dispatch needs if
`lie-scaling` applies. If the approach itself is still uncertain at this size, that's
`lie-brainstorming`'s job first — planning assumes the approach is already decided.

---

## Do

* state the actual goal in one sentence before anything else
* identify the simplest viable approach
* name what's affected and what verification will confirm it worked
* surface real risks, not hypothetical ones

## Don't

* restate information already visible in the repo
* explain code that's self-explanatory
* produce a plan document for Level 0/1 work
* spend more time planning than the task warrants
* treat the plan as fixed once reality disagrees with it — see Scope Lock in the
  `lightweight-iterative-engineering` skill for what to do when scope grows mid-task

---

## Principle

> **Plan enough to avoid the obvious mistake, then build.**
