# Testing

## Purpose

Provide testing guidance when a testing requirement is actually detected — not by default, and not by asking first.

Testing is a tool for confidence, not a mandatory development ceremony.

---

## Detecting a Testing Requirement (cheap-first)

Check signals in order of cost. Stop at the first hit — do not check further signals once one resolves the question, and do not explore beyond these two checks.

1. **Sibling file check (cheapest).**
   Does a test file already exist for the file/module being touched
   (e.g. `foo.test.ts` next to `foo.ts`, `test_foo.py` next to `foo.py`,
   a matching file under `__tests__/` or `tests/`)?
   → If yes: tests required. Follow that file's existing pattern. Stop here.

2. **Wired-in test command (one file read).**
   Does `package.json`, `pyproject.toml`, `Makefile`, or CI config
   (e.g. `.github/workflows/*.yml`) define a test script that already
   runs in CI or a pre-commit hook?
   → If yes: tests required for the changed behavior. Stop here.

3. **Neither signal found.**
   Treat this as "no established testing requirement" and proceed
   without tests. Do NOT keep exploring the repo to look for an
   unwritten convention — that exploration costs more than either
   writing the test or skipping it would have. This is the behavior
   this skill exists to prevent.

## When to Ask the User Instead of Deciding

Only ask if steps 1–2 both find nothing **and** the change is high-risk
per the parent skill's risk list (auth, authorization, cryptography,
payments, sensitive data, destructive operations, infrastructure, major
architectural changes, high-impact client-facing functionality).

For everything else: no signal found = proceed without tests, no
question asked.

---

## TDD

TDD is optional, even when testing is required.

Use:

```text
RED → GREEN → REFACTOR
```

when:

* behavior is well understood
* the test can meaningfully define the expected behavior
* the user or project requires TDD specifically (not just "tests")

Do not use TDD simply because it is available.

For exploratory work where expected behavior is still changing, it may be more efficient to implement first and test once the behavior is established.

---

## Test Scope

Prioritize meaningful tests:

1. requested behavior
2. important business logic
3. regression-prone behavior
4. important edge cases
5. integration boundaries

Do not generate tests merely to increase:

* test count
* coverage percentage
* perceived completeness

---

## Existing Tests

Respect existing project conventions.

Prefer extending existing test patterns over introducing new testing approaches.

Do not rewrite unrelated tests unless necessary.

---

## Verification

After writing tests:

* run the relevant tests
* investigate failures
* distinguish implementation failures from incorrect test assumptions
* update tests when requirements legitimately change

Do not hide or weaken a failing test simply to make the suite green.

---

## Principle

> **Detect cheaply. Test what matters when testing provides value. Don't ask when a five-second check already answers it.**