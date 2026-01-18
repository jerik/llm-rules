# Testing Policy

Goal: Maximize risk reduction per invested engineering hour.

## Decision Rule
Create a test only if:
```
(risk_impact × defect_probability) ≥ test_cost
```

## Prioritization Criteria
- High business impact
- High defect probability
- Low test complexity

## Test Type Preference
```
Unit > Integration > E2E
```
Minimal E2E tests (critical journeys only).

## Forbidden
- Coverage-driven tests without risk justification
- Redundant or flaky tests
- UI-only detail tests

## Ambiguity Handling
If requirements are unclear: state ambiguities explicitly, clarify with user before writing tests.
