---
name: flow-driven-tdd
description: Derive flow-first TDD tests from specs, using a reviewed flow inventory before writing tests.
---

## Use this skill when
- You need to derive tests from project specs before writing the tests.
- You need to work with the user flow-first TDD method agreed for this repo.
- You need to create or update the flow inventory before proposing test classes.

## Hard rules
- Before creating test classes, create or update `docs/testing-flow-inventory.md`.
- Do not create tests for a flow until that flow has been written in `docs/testing-flow-inventory.md` and reviewed by the user.
- Order the inventory from simpler flows to more complex flows.
- Keep `docs/testing-flow-inventory.md` concise and easy for a human to review quickly.
- Spend more effort validating that the proposed flows are correct than writing verbose inventory text.
- Prefer short, faithful reformulations over long prose copied from the specs.
- Start from user-visible flows/examples first.
- One top-level test class per flow/example.
- Follow the naming rules already established in `behavior-driven-java-tests`:
  - top-level class name describes the feature/action and ends with `Test`
  - `@Test` method names use `givenXWhenYThenZ`
  - `@Nested` names describe the step/action
- Inside each top-level class, model the flow step by step.
- Each step is a direct `@Test` unless that step has branches or failures to manage.
- If a step has branches or failures, represent that step with `@Nested`.
- Inside that `@Nested`, include:
  - one `@Test` for the ok case
  - one `@Test` for each non-ok case to manage
- Keep transversal errors inside the affected flow by default.
- Add internal/supporting flow classes only when a visible flow clearly justifies them.
- Keep support code separate from test classes.

## Test layout
- Put flow classes in `src/test/java/.../flow/`.
- Put end-to-end classes in `src/test/java/.../e2e/`.
- Put shared helpers in `src/test/java/.../support/`.
- Put reusable fixture builders/data setup in `src/test/java/.../support/fixtures/`.
- Put HTTP or process stubs in `src/test/java/.../support/stubs/`.

## Deliverable per flow
- Each approved flow should produce one top-level flow test class in `flow/`.
- Each approved flow should also produce one `...EndToEndTest` class in `e2e/`.
- The `...EndToEndTest` class should contain a single point-to-point test.

## Support emergence
- Introduce support helpers only when repetition makes them clearly useful.
- Put CLI launch helpers in `support/`.
- Put reusable scenario setup in `support/fixtures/`.
- Put external service stubs in `support/stubs/`.

## Error discipline
- Add failure cases when they come from the spec, follow clearly from the contract, or are unavoidable technical boundaries.
- Do not invent speculative edge cases just because they might exist.

## Verification discipline
- In red TDD phases, keep at least `./gradlew testClasses` green.
- Run `./gradlew test` when the goal is to execute the suite, not only to shape it.

## Core model
- Outside -> inside: specs and examples define the top-level test classes.
- Inside -> outside: the step-by-step flow defines the tests inside each class.
- The parent class comes from the flow/example.
- The child tests come from the steps needed to complete that flow.
- Error handling is expressed where it happens in the flow, not in detached error-only suites.

## Required workflow
1. Read the relevant specs.
2. Extract the candidate user-visible flows/examples.
3. Check that the flow list is faithful to the specs and remove weak or redundant entries.
4. Order the remaining flows from simpler to more complex.
5. Write them into `docs/testing-flow-inventory.md` using the concise review format.
6. Ask the user to review that inventory.
7. Wait for approval of the target flows.
8. Only after approval, derive the step-by-step tests for each approved flow.
9. Create one top-level class per approved flow.
10. Break each class into step tests.
11. Open `@Nested` only for steps with branches/failures.
12. Let implementation emerge by making those tests pass step by step.

## Inventory requirements
- `docs/testing-flow-inventory.md` must be the review point before test creation.
- Each inventory entry should use exactly this compact shape:
  - first line: `<id> - <short reformulated functionality line>`
  - second line: `Specs: ...`
  - third line: `Flujo: ...`
  - fourth line: `Clase propuesta: ...`
- Leave a blank line between entries.
- The first line should be a brief reformulation that is faithful to the specs, not a long literal quote.
- The `Flujo` line should stay short and human-readable.
- Do not put planned steps, `@Nested` blocks, priorities, statuses, notes, or review workflow prose into the inventory unless the user explicitly asks for them.

## Decision rules
- Prefer flows the user can see or trigger directly.
- Treat command-level or user journey behaviors as the default source of parent classes.
- Consider internal/support flows only when they are needed to support a visible flow cleanly.
- Do not organize the suite primarily by production class names.
- Do not skip the inventory review phase.

## Minimal structure
```java
class SomeFeatureTest {

    @Test
    void givenSomePreconditionWhenFirstStepThenExpectedResult() {
    }

    @Nested
    class SomeStepWithFailures {

        @Test
        void givenValidStateWhenStepRunsThenSucceeds() {
        }

        @Test
        void givenFailureCaseOneWhenStepRunsThenHandlesIt() {
        }

        @Test
        void givenFailureCaseTwoWhenStepRunsThenHandlesIt() {
        }
    }
}
```

## Relationship with other skills
- Use this skill to derive and structure the flow inventory and the parent/step test shape.
- Use `behavior-driven-java-tests` together with this skill when you actually write the JUnit 5 / Mockito tests.
