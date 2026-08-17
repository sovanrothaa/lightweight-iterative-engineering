# Lightweight Iterative Engineering (LIE)

A lightweight, **auto-activating**, full-lifecycle AI-assisted software engineering workflow —
focused on **proportional process** and minimal waste of both time and tokens.

LIE is a reaction to "ceremony-heavy" agent workflows that default to TDD on every task,
multi-agent review pipelines for trivial changes, hard approval gates on one-line fixes, and
clarifying questions where a five-second local check would answer instead. It keeps the parts
of that discipline that matter (mandatory review, evidence-based verification, systematic
debugging, an approval checkpoint on genuinely ambiguous work) and cuts the parts that don't
scale with task size.

> v0.5 — each `lie-*` capability skill is now individually discoverable (visible in skill
> listings) while still only firing through `lightweight-iterative-engineering`'s routing. v0.4
> introduced complexity-level routing (0-3) instead of a flat single-task loop, auto-activation
> at session start instead of requiring explicit selection, and full-lifecycle coverage
> (planning, brainstorming, testing, debugging, review, security, scaling, release checks).

---

## Origin

LIE's capability sub-skills are derived from [`obra/superpowers`](https://github.com/obra/superpowers)
(MIT licensed) — a widely used, battle-tested skills framework — with the ceremony trimmed for
proportional use. The mechanics that are genuinely proven (systematic debugging's phases,
parallel-dispatch rules, evidence-before-claims, the diff-scoped reviewer pattern) are kept; the
parts that make superpowers expensive by default for ordinary work (hard approval gates on every
task, per-task dual-agent review loops, mandatory spec-file ceremony) are not. See each
sub-skill's `SKILL.md` for what was kept and what was cut.

## Philosophy

The goal is not to follow every engineering best practice on every task. The goal is to use the
**minimum sufficient process** — not the minimum possible process — for the task at hand.

* **LIE is the workflow authority.** Once active, it decides whether planning, brainstorming,
  testing, deeper review, or a subagent dispatch is warranted — other installed skills don't get
  to impose their own process on top.
* **Routed by complexity, not by a flat checklist.** Level 0 (trivial) gets no ceremony. Level 1
  (normal) gets a lightweight plan and one review pass. Level 2 (complex/risky) gets an approach
  confirmation and thorough review. Level 3 (large/parallelizable) gets decomposition and one
  final whole-change review.
* **Testing is opt-in, not default.** Tests get written when explicitly requested or when a
  concrete signal (an existing sibling test file, a wired-in CI test command) says the project
  expects them.
* **A confirm-before-implementing checkpoint exists — but only for Level 2/3.** Superpowers'
  hard approval gate applies to every task including one-line fixes, which is a documented real
  complaint about it. LIE's version only fires when the approach is genuinely uncertain.
* **Ambiguity is resolved cheaply.** Prefer a fast local check or a reasonable default over
  stopping to ask — and prefer asking over open-ended exploration.
* **Subagent dispatch is scoped and cheap**, including tiering the reviewer's model to what the
  review actually requires — not defaulting every dispatch to the most expensive model available.
* **Verification means evidence**, not "the code looks right."

## What LIE Isn't

* **Not anti-testing** — tests when there's a concrete signal or an explicit request, not TDD by default.
* **Not anti-review** — review is mandatory, just proportional to risk.
* **Not anti-process for ambiguous work** — Level 2/3 still gets an approach check and thorough review.
* **Not for every project** — some teams genuinely need strict TDD or heavier process. LIE is a choice.

## Structure

```text
lightweight-iterative-engineering/
├── .claude-plugin/
│   ├── plugin.json
│   └── marketplace.json
├── hooks/
│   ├── hooks.json        # SessionStart -> auto-activation bootstrap
│   ├── session-start
│   └── run-hook.cmd      # Windows/POSIX polyglot wrapper
└── skills/
    ├── lightweight-iterative-engineering/SKILL.md   # entry point: workflow authority,
    │                                                 # complexity levels, routing, stop rules
    ├── lie-planning/SKILL.md
    ├── lie-brainstorming/SKILL.md   # Level 2/3 only: confirm the approach once, cheaply
    ├── lie-testing/SKILL.md
    ├── lie-debugging/SKILL.md
    ├── lie-review/SKILL.md
    ├── lie-security/SKILL.md
    ├── lie-scaling/SKILL.md         # subagent dispatch, model tiering, batching
    └── lie-release-gate/SKILL.md    # adaptive per-change verification checklist
```

Each `lie-*` capability is its own individually discoverable skill — so you can see exactly
which one fires, the same way superpowers' skills each show up separately — but every one of
them is scoped by its own `description` to being invoked by `lightweight-iterative-engineering`'s
routing, not independently. `lightweight-iterative-engineering` is the entry point and routes
into a capability only when the complexity level and task shape call for it — nothing fires
automatically just because it's visible in a skill list.

## Usage

Install as a Claude Code plugin:

```json
"extraKnownMarketplaces": {
  "lightweight-iterative-engineering": {
    "source": { "source": "github", "repo": "sovanrothaa/lightweight-iterative-engineering" }
  }
},
"enabledPlugins": {
  "lightweight-iterative-engineering@lightweight-iterative-engineering": true
}
```

## Activation

LIE auto-activates at session start via its `SessionStart` hook — no explicit selection
required. If the user explicitly selects a different workflow, LIE steps aside for that task.

Contextual phrases like "skip TDD," "keep the review lightweight," or "avoid unnecessary
ceremony" reinforce that LIE is the right fit; they aren't required to activate it.

## License

MIT — see [LICENSE](LICENSE).
