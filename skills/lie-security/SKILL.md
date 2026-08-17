---
name: lie-security
description: >
  LIE capability skill: concrete checklist for the security-specific subset
  of the shared risk list (auth, crypto, injection, PII, and similar
  triggers). Part of the lightweight-iterative-engineering (LIE) workflow —
  invoked by LIE's own routing when a triggered surface is touched, not on
  its own. If LIE is not already active for this task, invoke
  lightweight-iterative-engineering first instead of this skill directly.
---

# Security

## Purpose

Give the shared risk list (used by `lie-review` and `lie-testing`) a concrete checklist
for the security-specific subset of it, instead of leaving "security" as an unexamined line item.

---

## Trigger

Invoke when a change touches any of:

* authentication or authorization
* JWT / OAuth / session handling
* cryptography or secret handling
* database permissions or access control
* file uploads
* deserialization of untrusted data
* external/user-controlled input reaching a sensitive sink
* SSRF-sensitive behavior (server-side requests built from user input)
* payments or financial logic
* PII or other sensitive data
* SQL/NoSQL or other injection-prone surfaces

Ordinary CRUD work that doesn't touch the above does not need this — the shared risk list's
normal review depth is enough.

---

## Checklist

For triggered work, check:

* input validation at the boundary where untrusted data enters
* authorization — is the check actually enforced, not just present in intent
* output handling — does anything leak into a response, log, or error that shouldn't
* secure defaults — does the code fail closed, not open
* secret handling — no secrets in logs, error messages, or client-visible output
* injection — is input ever concatenated into a query, command, or interpreted string
* trust boundaries — does this code correctly treat the other side of the boundary as untrusted

Only check what the specific change actually touches — this is not a full security audit
triggered by every change, it's a targeted check for the surfaces above.

---

## Process

1. Identify what security impact the change could have.
2. Run the relevant checklist items above.
3. Verify secure behavior — evidence, not assumption (a passing auth check you actually ran,
   not "should be fine").
4. Note meaningful findings; fix what's in scope.
5. If a security decision can't be made safely without more context or carries real
   consequence if wrong (e.g. a genuinely ambiguous authorization boundary), escalate to the
   user rather than guess.

This escalates the change to at least the shared risk list's "high risk" review depth in
`lie-review` — it doesn't replace that review, it's the specific content of it for
security-triggered work.

---

## Principle

> **Check the boundary the change actually touches. Escalate what you can't verify.**
