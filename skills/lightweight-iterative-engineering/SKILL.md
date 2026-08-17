---
name: lightweight-iterative-engineering
description: >
  A full-lifecycle, proportional software engineering workflow for AI coding
  tasks: workflow authority, complexity-based routing (Level 0-3), and
  capability sub-skills for planning, brainstorming, testing, debugging,
  review, security, scaling to subagents, and release checks. Auto-activates
  at session start via hook. Use when the user wants lightweight or pragmatic
  engineering, minimal ceremony, conditional testing, no default TDD,
  lightweight review, or a proportional alternative to ceremony-heavy
  development workflows. Routes selectively based on task complexity and
  risk, not by applying every step to every task.
---

# Lightweight Iterative Engineering

## Purpose

LIE is the minimum sufficient engineering process for a given task — not the minimum possible
process, and not the maximum available one. Too little process produces defects; too much
wastes time and tokens for no added confidence. LIE finds the point where more process stops
paying for itself.

---

## Workflow Authority

When LIE is active, it decides the process for a task — whether planning, brainstorming,
testing, deeper review, or a subagent dispatch is warranted, and when the task is done. Other
installed skills or workflows may offer specialized techniques, but do not get to impose their
own process on top of LIE's once it's active — that's how ceremony compounds when skills stack.
LIE's own sub-skills (`planning/`, `brainstorming/`, `testing/`, `debugging/`, `review/`,
`security/`, `scaling/`, `release-gate/`) are capabilities LIE routes to deliberately, not
independent triggers.

> **Never perform a process step because the framework can. Perform it because the task
> benefits from it.**

---

## Activation

LIE auto-activates at session start (see the plugin's `SessionStart` hook) as the default
workflow authority for coding tasks. If the user explicitly selects a different workflow,
follow that instead — LIE does not fight for control once someone has chosen something else.

Contextual signals that reinforce LIE is the right fit: "skip TDD," "minimal process,"
"don't over-engineer," "keep the review lightweight," "avoid unnecessary ceremony" — these
confirm, they aren't required to activate it.

---

## Complexity Classification

Before routing, size the task on four dimensions — this should be a fast judgment call, not a
ceremony of its own:

* **Scope** — how large is the actual change?
* **Risk** — how costly is an incorrect implementation?
* **Uncertainty** — how well understood is the problem and the desired solution?
* **Coordination** — how many systems, interfaces, or independent workstreams are involved?

Do not classify primarily by file count — it's evidence, not the answer. A 40-file mechanical
rename can be Level 0/1; a one-line authorization change can be Level 2; three independent
service implementations can be Level 3.

**Level 0 — Trivial.** Typo, rename, obvious config change, mechanical fix with negligible
risk. No plan, no test, no review ceremony beyond a self-check.

**Level 1 — Normal.** Ordinary bug fix, small endpoint, isolated feature, moderate config
change. Lightweight plan, test-after by default, one lightweight review pass.

**Level 2 — Complex.** Auth/authorization changes, database migrations, significant business
logic, multi-service features, architectural changes, high-risk production behavior. Approach
may need confirming first (`brainstorming/`), TDD is worth considering, review is more
thorough, `security/`'s trigger list likely applies.

**Level 3 — Large / Parallelizable.** Many independent migrations, multiple independent
services, a feature spanning systems. Decompose, consider `scaling/` for subagent dispatch,
integration verification is mandatory, one final whole-change review.

The ratchet is one-way within a task: hidden complexity discovered mid-task can upgrade the
level (stop, say so, re-route). Nothing downgrades mid-task just because it's mostly done.

---

## Budget Awareness

Soft, behavioral targets — not an accounting exercise. Don't spend tokens computing exact
consumption per phase.

| Level | Tokens | Time |
|---|---|---|
| 0 | < 2K | < 1 min |
| 1 | < 15K | < 5 min |
| 2 | < 40K | < 15 min |
| 3 | < 100K | < 30 min |

When approaching a target: stop unnecessary work, reassess scope, take the cheapest useful next
action, avoid speculative improvements, split the task if that's the actual fix. Do not
automatically abort a legitimate task just because a soft target was crossed — the target is a
prompt to reassess, not a hard stop.

---

## Shared Risk List

Reused by `testing/`, `review/`, and `security/` — one taxonomy, not a separate one per
sub-skill: authentication/authorization, cryptography, payments, sensitive data/PII, destructive
operations, infrastructure, significant architectural changes, high-impact client-facing
functionality, and anything else where failure has substantial consequence.

---

## Routing

| Level | Flow |
|---|---|
| 0 | inspect → change → self-check → done |
| 1 | understand → `planning/` (3-5 bullets) → implement → `testing/` if signaled → `review/` (lightweight) → done |
| 2 | understand → `brainstorming/` if the approach is genuinely uncertain → `planning/` → implement → `testing/` (TDD worth considering) → `review/` (thorough) → `security/` if triggered → done |
| 3 | `planning/` → decompose → `scaling/` if subagent dispatch pays for itself → execute → integration verification → one final whole-change `review/` → done |

Debugging: route to `debugging/` whenever the primary task is diagnosing a defect, at whatever
level the fix itself turns out to be. Release checks: route to `release-gate/` before declaring
any Level 1+ task done — it picks the applicable subset of build/test/lint/migration/security
checks rather than running everything or nothing.

---

## Escalation

Ask the user when — not by default, only when:

1. requirements are genuinely ambiguous and a cheap local check (existing file, config,
   pattern) doesn't resolve it
2. multiple materially different approaches have no clear winner
3. a security-sensitive decision can't be made safely without more context
4. scope must materially increase beyond what was agreed
5. risk is high and real uncertainty remains

Investigate first if repository evidence can resolve it — a clarifying question and a
speculative exploration pass both cost a round trip; pick whichever actually resolves it
cheaper, and prefer neither when a fast local check already answers it.

Keep escalation concise: state the issue, 2-3 options (not five speculative ones), a
recommendation, and the impact of each — then wait.

---

## Stop Rules

Stop when acceptance criteria are met, verification provides sufficient evidence, no blocking
issues remain, and no unresolved decision needs the user. Do not: search indefinitely for
hypothetical problems, refactor unrelated code, add tests solely for coverage, improve
already-correct code without a requirement, or keep reviewing after actionable issues are
exhausted. A completed task ends — it doesn't get polished indefinitely.

---

## Scope Lock

Don't expand a task because something unrelated was discovered along the way. Complete the
requested work, verify it, mention the unrelated finding, stop. Expand scope only when it's
required for correctness, security, or verification of the actual task — or the user explicitly
asks.

---

## State (`.lie/state.md`)

Optional — create only when a task genuinely spans sessions or context is degrading enough that
losing track of decisions would cost more than the file does. Not for Level 0/1 work, and not a
default for every Level 2/3 task either.

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
chain-of-thought, or output easily re-derived by re-reading the repo. Before clearing context on
a multi-session task: update the file. After: read it, inspect current repo state, continue.

---

## Completion

Work is done when: the requested behavior is implemented, it fits the existing project,
required verification ran (`release-gate/`), review happened at the appropriate depth, tests
were written if signaled or requested, a final whole-change review ran for multi-task work, and
no unintended changes remain. If the work lives on a branch, confirm the base branch and offer
the real integration options — merge now, push and open a PR, or leave it as-is — don't assume
merge just because the diff looks done, and don't discard anything without an explicit request.

Scale the completion response to the task: a Level 0/1 fix gets a sentence or two with what was
verified; Level 2/3 work gets changes, verification, review outcome, and any remaining risk —
never "looks good" without the evidence behind it.

---

## Core Rule

> **Build what's needed. Confirm the approach once, cheaply, when it's genuinely uncertain.
> Test when a real signal says to. Review every change, proportionally. Resolve ambiguity with
> the cheapest check that actually answers it. Harden the whole result once — not repeatedly.**
