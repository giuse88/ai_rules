## Working agreement
- Before writing code, summarise your plan and wait for approval.
- Work in small steps. After each one, run `python -m pytest` and show the result.
- When you find behaviour in existing code that looks wrong, stop and report it with a failing test. Don't silently fix it as part of an unrelated change.
- Never edit existing tests to make them pass. If one seems wrong, say so and ask.
- Use an additional agent to review your code. This agent should follow the same agreement specified in this file

## Conventions
- Python 3.10+ (so `ts` parsing must handle the `Z` suffix manually). Work inside the project venv,
  not the system interpreter.
- Use Ruff (config in `pyproject.toml`, line length 100).
- Favor iterating toward simpler, less verbose code, but never at the expense of performance.
- When code or an interface is genuinely complex, add a concise plain-English comment explaining the *why*, not the *what*.
- Add or extend `Makefile` targets rather than introducing a separate task runner.

## Testing
- `tests/units/`: pytest unit tests for individual functions and classes.
- `tests/integration/`: pytest tests that call on temporary CSVs and assert on
  stdout, stderr and the exit code.
- `tests/acceptance/`: behavioral tests. An agentic AI runs the installed CLI and verifies the
  requirements R1–R11 in `tests/acceptance/README.md`. Each `scenarios/<name>/` contains
  `input.csv` and a hand-computed `expected.csv`. Derive new expected outputs by hand, never by
  copying the tool's output. Do not import internals here.
  - Use parametrize tests where possible to keep the amount of code written small
  - Reiterate over the test set to identify duplcates as code evolves over time, but make sure to mantain
    the same code coverage and make sure to cover the same logic and edge cases. 

## Commits
- Work on a branch: `git switch -c feat/<name>` before the first change.
- One logical change per commit. Never mix a bug fix, a refactor, formatting and a feature.
- Order: failing test for a bug → fix → refactor (no behaviour change) → feature → docs.
- Every commit must pass `python -m pytest`. Run it before committing and show the result.
- Stage explicitly (`git add <paths>` or `git add -p`), never `git add -A`.
- Message: imperative subject ≤ 72 chars, blank line, body explaining *why*.
  Prefix: fix:, feat:, refactor:, test:, docs:, chore:.
- Don't commit until I've approved the step. Never amend, rebase or force-push without asking.
- Don't reformat existing code. Run formatters only on files or lines you changed.
