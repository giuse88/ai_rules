# CLAUDE.md

## Commands
```bash
make install      # create .venv and install with dev extras
make test         # all tests (pytest)
make lint         # ruff check + format check
make run INPUT=…  # run the CLI on a file
```
Always use these targets. Work inside the project venv, not the system interpreter.

## Source of truth
- `SPEC.md` defines the required behaviour. If something is ambiguous or missing, ask. Don't assume.
- Priority: correctness > clarity > performance, unless the spec says otherwise.
- Data models before the logic that uses them
- Core functionality before edge cases
- Bug fixes before new features that depend on the broken code working reiterate over the code

## Working agreement
- Before writing code, summarise your plan **and the commit sequence** (one line per commit, each
  green). Wait for approval.
- Work in small steps. After each one, run `make test` and show the result.
- Keep diffs minimal. Don't add dependencies, tools or files outside the approved plan without
  asking.
- When you find behaviour in existing code that looks wrong, stop and report it with a failing
  test. Don't silently fix it as part of an unrelated change.
- Never edit existing tests to make them pass. If one seems wrong, say so and ask.
- If an approach fails twice, stop and report what you tried and what you suspect.

## Review
- Before the final commit, run **one** separate review agent on the full diff against `SPEC.md`.
- The reviewer is **read-only**: it reports issues ranked by severity (bug, spec gap, quality) and
  changes nothing. I decide what to act on.

## Conventions
- Python 3.10+. Standard library only at runtime unless the spec says otherwise.
- Ruff for linting and formatting (config in `pyproject.toml`, line length 100).
- Iterate toward simpler, less verbose code, without regressing performance.
- When code or an interface is genuinely complex, add a concise comment explaining the *why*, not
  the *what*.
- Public functions have type hints and a one-line docstring. Keep internals private (leading
  underscore).
- Add or extend `Makefile` targets rather than introducing a separate task runner.

## Testing
- `tests/unit/`: pytest unit tests for individual functions and classes.
- `tests/integration/`: tests that run the CLI on temporary inputs and assert on stdout, stderr and
  the exit code.
- `tests/acceptance/`: one parametrized pytest that runs the installed CLI on each
  `scenarios/<name>/input.csv` and diffs its output against `expected.csv`. Use only the public
  CLI here; don't import internals.
- **`expected.csv` files are written by me. Never create or modify them.** If you think one is
  wrong, say why and ask.
- Use `pytest.mark.parametrize` to keep test code small.
- New code needs tests. Every requirement in `SPEC.md` must map to at least one test.

## Commits
- Work on a branch: `git switch -c feat/<name>` before the first change.
- One logical change per commit. Never mix a bug fix, a refactor, formatting and a feature.
- For a bug: failing test → fix. Otherwise: refactor (no behaviour change) → feature → docs.
- Every commit must pass `make test`. Run it before committing and show the result.
- Stage explicitly (`git add <paths>` or `git add -p`), never `git add -A`.
- Message: imperative subject ≤ 72 chars, blank line, body explaining *why*.
  Prefix: `fix:`, `feat:`, `refactor:`, `test:`, `docs:`, `chore:`.
- Don't commit until I've approved the step. Never amend, rebase or force-push without asking.
- Don't reformat existing code. Run formatters only on files or lines you changed.
