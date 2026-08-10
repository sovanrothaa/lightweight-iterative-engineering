# Review

## Purpose

Review every completed implementation before considering it done.

The default review is intentionally lightweight.

Review exists to catch meaningful problems—not to create review artifacts.

---

## Default Review

Perform one focused review pass.

Check:

* Does the implementation satisfy the request?
* Are there obvious correctness problems?
* Are there unintended changes?
* Does it follow existing project patterns?
* Is there unnecessary complexity?
* Are important error cases handled?

Prefer a quick self-review or one reviewer pass.

Do not automatically create:

* multiple review agents
* separate review packages
* lengthy reports
* repeated review cycles

---

## Risk-Based Escalation

Escalate review when the change involves meaningful risk.

This is the same risk list used elsewhere in LIE (e.g. the testing sub-skill's ask-the-user exception) — one taxonomy, reused, not redefined per sub-skill.

### Medium risk

Examples:

* complex business logic
* significant API changes
* database changes
* cross-module changes
* large refactoring

Perform a more thorough review.

### High risk

Examples:

* authentication
* authorization
* cryptography
* payments
* sensitive data
* infrastructure
* destructive operations
* major architecture changes
* high-impact client-facing functionality

Use deeper or independent review when justified.

---

## Final Whole-Change Review

For multi-task work, perform one additional whole-change review after all tasks are complete.

Look specifically for:

* cross-task conflicts
* integration issues
* inconsistent assumptions
* duplicated solutions
* architectural drift
* missing end-to-end behavior
* unintended interactions

This is one review across the complete change.

Do not perform a heavy whole-change review after every individual task.

---

## Independent Review

A second independent reviewer is optional.

Use one only when:

* risk is high
* the first review identifies substantial issues
* independent analysis materially increases confidence
* the user requests it

Do not use dual review as the default.

---

## Findings

Prioritize findings by impact.

Focus on:

1. correctness
2. security
3. regressions
4. important maintainability problems
5. significant architectural issues

Avoid reporting inconsequential stylistic preferences as defects.

When possible, verify findings with evidence before reporting them.

---

## Fixing Findings

When a review identifies an issue:

1. Fix the issue.
2. Run relevant verification.
3. Re-check the affected area.

Do not restart the entire review process unless the changes materially alter the implementation.

---

## Principle

> **Review to find meaningful problems, not to prove that a review happened.**