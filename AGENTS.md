# AGENTS.md

Guardrails for any agent or human working in this repo. Keep this file short; put detail in `docs/`.

## Stack

Vite+, TypeScript (strict), React, npm, Tailwind CSS.

- Use npm only (no yarn/pnpm). Commit `package-lock.json`.
- Keep dependencies up to date: check `npm outdated` at the start of a task, update in small separate commits, and run tests after. Record major upgrades in `docs/decisions.md`.
- Don't add a dependency without a reason; prefer the platform and existing deps first.

## Workflow (read before starting, update before finishing)

1. Read `docs/progress.md` (where we left off), then `docs/roadmap.md` and `docs/architecture.md`.
2. Do the task in small, reviewable steps.
3. A task is **not done** until:
   - tests for the new or changed behavior exist and pass,
   - lint, typecheck and build pass,
   - `docs/progress.md` says what was done, what is next, and any open problems,
   - any decision made along the way is in `docs/decisions.md`.

## Docs (`docs/`)

| File | Purpose |
|--|--|
| `architecture.md` | Current structure and boundaries. Links to `docs/architecture/<feature>.md` for feature-level detail. |
| `roadmap.md` | Planned work, ordered by priority. |
| `gaps.md` | Known shortcomings, tech debt, missing tests, unresolved questions. |
| `progress.md` | Log of finished work and the exact next step. Newest entry first. |
| `decisions.md` | One entry per decision: context, choice, alternatives, consequences (ADR style). |

### Decisions

Every non-trivial decision (library, pattern, structure, trade-off) gets an entry. If code and docs disagree, fix the docs in the same change.

**Decision format (lean & readable):**
- **Context:** Why are we making this decision?
- **Decision:** The choice made.
- **Alternatives:** What was considered and rejected (1–2 lines each).
- **Consequences:** Trade-offs and what we gain/lose.

### Requirements engineering (grill-me)

Every new feature or significant change is challenged with the `grill-me` skill (`.agents/skills/grill-me/SKILL.md`) **before** any code is written:
1. **Problem:** What is solved, for whom, and what is the simplest alternative?
2. **Architecture fit:** Does it fit `docs/architecture.md` and the feature boundaries? Does it add coupling?
3. **Conflicts:** Does it contradict an existing feature, requirement, or decision? Check `roadmap.md`, `docs/architecture/`, and `decisions.md`.
4. **Testable:** Are there acceptance criteria that can be verified by a test?

Then record the result: requirements go in `docs/roadmap.md`, or in `docs/architecture/<feature>.md` when they need detail; the outcome, including "proceed as planned", goes in `docs/decisions.md`. If the challenge fails, stop and ask the user.

**Docs stay in sync:** a change is not done while roadmap, architecture, and code disagree. Update docs in the same change.

## Architecture rules

- Organize by feature (`src/features/<name>/`), not by file type. Shared code lives in `src/shared/`.
- Features don't import from each other's internals; expose a small public API (`index.ts`).
- Keep UI components presentational; put logic in hooks or plain TS modules that can be tested without React.
- Isolate external I/O (API, storage) behind a thin module so it can be mocked.
- No `any`; validate data at system boundaries (network, storage, user input).
- Prefer simple over clever. No speculative abstractions; extract on the third repetition.

## Testing

- Every new feature and bug fix comes with tests; a bug fix starts with a failing test.
- Test behavior, not implementation details. Co-locate tests with the code (`*.test.ts` next to `*.ts`).
- Never skip or delete a failing test to get green.

## Security

- Never put secrets (keys, tokens, passwords) in source, tests, docs, or commit history.
- Read config from environment variables; commit only `.env.example` with placeholder values. `.env*` stays git-ignored.
- Note: anything prefixed `VITE_` is shipped to the browser, so it is public by definition.
- If a secret is committed by accident, tell the user so it can be rotated; don't just delete it.

## Skills

When a feature pattern repeats (new feature scaffold, new route, new API client), capture it as a skill (`SKILL.md` with frontmatter) instead of re-explaining it here. Skills live in `.agents/skills/`; `.claude/skills` is a symlink to it. Agents without skill support: read the `SKILL.md` file directly when its description matches the task. Mention new skills in `docs/architecture.md`.

## Don't

- Don't make changes outside the scope of the task; log them in `docs/gaps.md` instead.
- Don't leave TODOs without a matching entry in `docs/gaps.md`.
