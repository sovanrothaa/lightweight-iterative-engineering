# Lightweight Iterative Engineering (LIE) (work-in-progress)

A lightweight AI-assisted software engineering skill focused on **fast iteration, proportional process, and minimal waste** — of both time and tokens.

LIE is a reaction to "ceremony-heavy" agent workflows that default to TDD on every task, multi-agent review pipelines for trivial changes, and clarifying questions where a five-second local check would answer the question instead. It keeps the parts of that discipline that matter (mandatory review, evidence-based verification, systematic debugging) and cuts the parts that don't scale with task size.

> v0.1 — early and unpolished. Structure and rules will change as it gets used on real tasks.

---

## Philosophy

The goal is not to follow every engineering best practice on every task. The goal is to use the **minimum process necessary to produce correct, maintainable software.**

Concretely, that means:

* **Testing is opt-in, not default.** Tests get written when explicitly requested or when a cheap, concrete signal (an existing sibling test file, a wired-in CI test command) says the project expects them — not because code was written.
* **Review is mandatory but proportional.** Every change gets one lightweight review pass. Deeper review is reserved for genuinely risky work (auth, payments, crypto, infra, destructive operations).
* **Ambiguity is resolved cheaply.** Prefer a fast local check or a reasonable default over stopping to ask the user — and prefer asking over open-ended exploration, since aimless exploration can cost more than the question would have.
* **Multi-task work gets one final whole-change review**, not a heavy review repeated after every sub-task.
* **Verification means evidence**, not "the code looks right."

## What LIE Isn't

* **Not anti-testing** — LIE tests when there is a concrete signal (such as existing test files or CI test commands) or when testing is explicitly requested. It simply does not default to TDD on every task.
* **Not anti-review** — Review is mandatory. It is simply proportional: lightweight by default, deeper when risk justifies it.
* **Not for every project** — Some teams and codebases require strict TDD or heavyweight engineering processes. LIE is a choice, not a prescription.
* **Not a silver bullet** — LIE is a workflow, not a substitute for good engineering judgment.


## Structure

```text
lightweight-iterative-engineering/
├── SKILL.md                  # Core workflow, principles, routing
└── sub-skills/
    ├── testing/SKILL.md      # When and how to test
    ├── review/SKILL.md       # Lightweight review, risk-based escalation
    └── debugging/SKILL.md    # Reproduce → investigate → root cause → fix → verify

```

`SKILL.md` is the entry point and routes into the three sub-skills only when there's a concrete reason to — debugging when the task is a defect, testing when a signal is found, review always (but scaled to risk).

## Who this is for

Anyone using an AI coding agent (Claude Code, or similar) who wants a workflow that defaults to "do the smallest correct thing and verify it," rather than one that defaults to generating tests, reports, and review documents for every change regardless of size.

## Usage

Drop the `lightweight-iterative-engineering/` folder into wherever your tool loads skills from (e.g. a `skills/` directory your agent is configured to read). The parent `SKILL.md` is designed to be loaded first; it references the sub-skills by path and loads them only when its routing rules call for it.

## Activation

LIE is activated when the user explicitly selects the workflow, for example:

- "Use LIE for this task."
- "LIE mode."
- "Use Lightweight Iterative Engineering."
- "Use the lightweight engineering workflow."

Contextual requests such as "skip TDD", "keep the review lightweight", or
"avoid unnecessary ceremony" may also indicate LIE when the surrounding
request clearly calls for a lightweight engineering workflow.

When LIE is active, it controls the development workflow for the current task.
Other installed skills may still be used as supporting capabilities, but their
conflicting workflow rules should not be applied automatically.

LIE does not assume it is active when the user has not selected it.

## License

MIT — see [LICENSE](LICENSE).
