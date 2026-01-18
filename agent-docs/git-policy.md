# GIT POLICY
Apply these rules to all Git operations in this repository.

## Branch Strategy
- **Main**:
  - MUST be protected.
  - MUST NOT receive direct commits or pushes.
  - MUST only be updated via Pull Requests.
- **Feature Branches**:
  - MUST branch from `main`.
  - MUST follow naming: `feature/<short-description>` or `fix/<short-description>`.
  - SHOULD be small, focused, and short-lived.

## Commit Messages
- Language: **English only**.
- Format: **Conventional Commits** strongly preferred.
  - Types: `feat`, `fix`, `docs`, `chore`, `refactor`, `test`, `ci`
- Rules:
  - Use imperative mood.
  - One logical change per commit.
  - Avoid vague messages (e.g. “update”, “changes”).
- Examples:
  - `feat: add user authentication`
  - `fix: handle null input in parser`

## Pull Requests
- MUST include a clear description of *what* and *why*.
- SHOULD reference related issues or tickets.
- MUST pass CI checks before merge.
- SHOULD be reviewed by at least one other person.

## Code & History Hygiene
- Code and comments MUST be in English.
- Keep history readable: prefer small commits.
- Use rebase or squash when merging if required by the project.

## Safety Rules
- Never commit secrets or credentials.
- If history must be rewritten, explain and coordinate first.
