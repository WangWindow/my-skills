---
name: verified-incremental-development
description: Use when changing an existing application and the work should remain small, testable, reviewable, and safely integrated into the main branch
---

# Verified Incremental Development

Use this style for maintenance and feature work in an existing codebase. The goal is a small, observable change with a clean handoff—not a large batch of speculative edits.

## Core loop

1. **Establish context.** Check repository status, recent history, project instructions, relevant files, and the current test/build baseline. Preserve unrelated user changes.
2. **Bound the task.** Define one outcome, acceptance checks, files likely to change, and explicit non-goals. If behavior or UX is ambiguous, present a short design and get approval before coding.
3. **RED.** Add the smallest behavior or regression test first. Run it and confirm it fails for the intended reason.
4. **GREEN.** Implement the smallest production change that makes the test pass. Avoid opportunistic refactors and new configuration formats.
5. **Verify.** Run the targeted test, then the relevant full test suite, linter/type checker, and build. Report command evidence, not assumptions. For performance work, add a representative large-workload test and do not claim a speedup without measurements.
6. **Integrate.** Create a short-lived branch from `main` (without a worktree when the user wants editor visibility), make one commit for the complete task, fast-forward merge it into `main`, delete the branch, and confirm a clean status. Use a [Conventional Commit](https://www.conventionalcommits.org/) header such as `fix(scanner): reuse the import session`; inspect `git log --oneline` when matching an established repository style. For substantial changes—such as refactors, multi-module edits, dependency updates, migrations, or behavior/API changes—add a body after a blank line explaining what changed and its impact, using bullets when useful. For small, single-purpose changes, the body may be omitted when the header is sufficiently descriptive. Use English by default, unless the user or repository convention specifies another language. Push only when explicitly requested.

## Boundaries and safety

- One task means one coherent commit; do not split a small fix into progress or WIP commits.
- Use English by default for commit messages and new documentation/comments unless the user or repository convention requires another language.
- Always include a Conventional Commit header. Prefer a message body for substantial changes; omit it only for small, self-explanatory changes where the header provides enough context.
- Do not casually add generated, migration, temporary, intermediate, scratch, secret, or `docs/superpowers` files; keep changes in the smallest necessary set of existing or explicitly requested files.
- Keep public APIs and event formats stable unless the task requires a reviewed change.
- For bugs, trace the symptom to its cause before editing. For behavior changes, use test-driven development.
- If verification exposes an unrelated failure, distinguish it clearly instead of hiding or rewriting it.

## Completion report

State the user-visible result, important files, tests and builds that actually passed, commit/merge status, and the next bounded task. Mention anything not measured or not manually verified.
