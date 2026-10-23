---
name: grill-me
description: Challenge a new feature or significant change before any code is written. Use whenever a feature is requested, a requirement changes, or a design choice is made.
---

# Grill-me

Interrogate the request before implementing. Ask the user one question at a time when an answer is missing; don't guess.

## Checklist

1. **Problem:** What problem does this solve, for whom? What is the simplest alternative (including doing nothing)?
2. **Architecture fit:** Read `docs/architecture.md`. Does it fit the feature boundaries? Does it add coupling or a new dependency?
3. **Conflicts:** Check `docs/roadmap.md`, `docs/architecture/`, and `docs/decisions.md`. Does it contradict an existing feature, requirement, or decision?
4. **Testable:** Write acceptance criteria that a test can verify. If you can't, the requirement is unclear.
5. **Scope:** What is explicitly out of scope? Log deferred items in `docs/gaps.md`.

## Outcome

- Requirements go in `docs/roadmap.md`, or `docs/architecture/<feature>.md` if detailed.
- Record the result in `docs/decisions.md` (Context, Decision, Alternatives, Consequences), including "proceed as planned".
- Update `docs/architecture.md` if structure changes.
- If any check fails and can't be resolved, stop and ask the user.
