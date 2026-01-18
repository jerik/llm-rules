# Git Policy

Apply these rules to all Git operations in this repository.

## Branch Strategy
- **main**: Protected, no direct commits, update only via Pull Requests
- **Feature Branches**: 
  - Branch from `main`
  - Naming: `feature/<short-description>` or `fix/<short-description>`
  - Keep small, focused, and short-lived

## Commit Messages
- Language: English only
- Format: Conventional Commits
  - Types: `feat`, `fix`, `docs`, `chore`, `refactor`, `test`, `ci`
- Rules:
  - Imperative mood
  - One logical change per commit
  - No vague messages ("update", "changes", "fix")
- Examples:
  - `feat: add user authentication`
  - `fix: handle null input in parser`

## Pull Requests
- MUST: Include clear description of *what* and *why*
- SHOULD: Reference related issues or tickets
- MUST: Pass CI checks before merge
- SHOULD: Be reviewed by at least one person

## Safety Rules
- NEVER commit secrets or credentials
- Coordinate before rewriting history
