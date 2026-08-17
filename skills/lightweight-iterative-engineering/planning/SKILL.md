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
`../scaling/SKILL.md` applies. If the approach itself is still uncertain at this size, that's
`../brainstorming/SKILL.md`'s job first — planning assumes the approach is already decided.

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
* treat the plan as fixed once reality disagrees with it — see Scope Lock in the root `SKILL.md`
  for what to do when scope grows mid-task

---

## Principle

> **Plan enough to avoid the obvious mistake, then build.**
