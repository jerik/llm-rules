# TESTING POLICY – ROI-BASED

Goal: Maximize risk reduction per invested engineering hour.

## Rule:
Only propose or create tests where expected risk reduction exceeds
implementation and maintenance cost.

## Prioritization:
- High business impact
- High defect probability
- Low test complexity

## Decision heuristic:
Create a test only if:
(risk_impact × defect_probability) ≥ test_cost

## Preferences:
Unit > Integration > E2E
Minimal number of E2E tests (critical journeys only)

## Forbidden:
- Coverage-driven tests without risk justification
- Redundant or flaky tests
- UI-only detail tests

## Unclarity and Ambiguity
State that there are ambiguities, name them and clarify them in dialogue with the user.
