# Agent Instructions

## Communication
- MUST: Ask clarifying questions before guessing or assuming
- MUST: Ask questions immediately, not after implementation
- MUST: Be factual, critical, and precise
- MUST: Keep responses concise unless explicitly asked for detail
- MUST: Number questions as f1, f2 (max 2 per round)
- MUST: Each question must build on user's previous answer
- SHOULD: Formulate questions in TL;DR style (max 3-4 lines)

## Critical Analysis
- MUST: Critically analyze content and prompts
- MUST: Name weaknesses or contradictions
- SHOULD: Suggest improvements (mark as suggestions)

---

# Workflow

## Overview

```
Phase 1 (always) → Size Classification → Phase 2/3/4 (based on size)
```

| Size | Criteria | Phases |
|------|----------|--------|
| **XS** | Typo, config change, trivial fix, 1-2 files | Phase 1 → Phase 3 |
| **S** | Small feature, <50 LOC, single component | Phase 1 → plan.md → Phase 3 |
| **M** | New feature, multiple components, 50-200 LOC | Phase 1 → plan.md → Phase 3 → Phase 4 |
| **L/XL** | Architecture change, new module, >200 LOC | Phase 1 → Phase 2 → Phase 3 → Phase 4 |

User can override with: `use [SIZE]` or `use full workflow`

---

## Phase 1: Discovery & Clarification (always)
- MUST: Read userstory.md if present, otherwise ask user for the task
- MUST: Read readme.md, documentation.md, lessonslearned.md if present
- MUST: Clarify task in dialogue, update userstory.md
- **GATE:** Task is clear and approved by user
- **THEN:** Classify task size, announce: `[SIZE] <brief reasoning>`

---

## Phase 2: Solution Design (L/XL only)
- MUST: Read all necessary files to understand the application
- MUST: Create solutiondesign.md containing:
  - Structured requirements
  - As-is analysis and context
  - 2-3 solution options with decision matrix (criteria, weighting, recommendation)
  - Architecture and approach
  - Security aspects
  - Implementation and test considerations
- MUST: Clarify solutiondesign.md in dialogue until final
- MUST: Create plan.md with implementation phases and milestones
- **GATE:** solutiondesign.md and plan.md complete and approved by user

---

## Phase 2b: Planning (S/M only)
- MUST: Create plan.md with implementation steps
- **GATE:** plan.md approved by user

---

## Phase 3: Implementation & Test (always)
- MUST: Follow plan.md during implementation (if exists)
- MUST: Update plan.md status during implementation (if exists)
- MUST: Follow principles: KISS, DRY, YAGNI
- MUST: Develop in small increments, commit often with meaningful messages
- MUST: Create tests for implemented code (see testing-policy.md for ROI decision)
- MUST: Iterate until all tests pass
- MUST: Run linting and formatting when complete, then re-run tests
- MAY: Use TDD when appropriate
- **GATE:** Implementation complete, user informed and approved

---

## Phase 4: Reporting & Review (M/L/XL only)
- MUST: Update project documentation (readme.md, documentation.md, lessonslearned.md)
- MUST: Update solutiondesign.md if changes occurred during implementation (L/XL)

---

# Progressive Disclosure

The following documents contain task-specific guidelines.  
Do NOT read files blindly. Decide which are relevant for the current task.

## Instruction Index

### Git Policy
File: `agent-docs/git-policy.md`

Read when:
- Creating commits
- Working with branches
- Creating pull requests

### Python Policy
File: `agent-docs/python-policy.md`

Read when:
- Creating or modifying Python code
- Setting up Python projects

### Testing Policy
File: `agent-docs/testing-policy.md`

Read when:
- Writing or improving tests
- Deciding whether to write tests
