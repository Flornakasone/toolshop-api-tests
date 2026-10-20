# Contributing

## Workflow
1. Update `main`: `git switch main && git pull`
2. Create a branch: `git switch -c type/short-description`
3. Commit using Conventional Commits
4. Push and open a Pull Request (the template loads automatically)
5. Self-review the diff in "Files changed"
6. Merge and delete the branch

> `main` is protected: all changes go through a Pull Request.

## Branch naming
Format: `type/short-description` (lowercase, words separated by `-`)

Examples:
- `test/chained-crud`
- `ci/github-actions`

## Commit messages
Format: `type: short description` (imperative, lowercase, under ~50 characters)

| Type | When to use |
|------|-------------|
| `test` | add or change tests |
| `fix`  | fix something broken |
| `ci`   | pipeline or automation |
| `docs` | documentation |
| `chore` | maintenance and configuration (not tests, CI or docs) |

Example: `test: add schema validation for products`