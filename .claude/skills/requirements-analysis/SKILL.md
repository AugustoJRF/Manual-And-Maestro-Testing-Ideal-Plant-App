---
name: requirements-analysis
description: Use when reviewing requirements or user stories before writing test cases. Checks clarity, testability, contradictions, missing cases and scope, and produces questions for the product owner.
---

# Requirements analysis

Goal: find problems in the requirements before test cases are written, and turn each one into a question for the product owner, instead of assuming the answer.

## Input

- The requirements or user stories to review
- The designs or screens, if they exist
- Related requirements of other screens

## Steps

Check each requirement for:

1. **Clarity.** Vague words that cannot be tested: "fast", "easy", "appropriate", "at the top". Ask for the exact value, text or position.
2. **Testability.** Can a test case end with a Then that can be checked? If not, the requirement needs a measurable result.
3. **Consistency.**
   - Does it contradict another requirement?
   - Does it match the screens? Text, position and states must be the same in both.
4. **Missing cases.** What the requirement does not say, but users will do:
   - boundary values (minimum, maximum, zero, one above the limit);
   - empty states, errors and invalid input;
   - both platforms: the Android system back (button or swipe) and the iOS swipe back gesture;
   - what happens after the action (navigation, data saved, what is reset).
5. **Data.** Where the data comes from (back office, API) and what the app does with each value it can receive.
6. **Scope.** What is visible but not specified. Decide with the team: specify it now, or list it as out of scope.

## Output

A table of findings, one row per problem:

| # | Requirement | Type | Finding | Question or suggestion | Decision |
|---|---|---|---|---|---|

- **Type:** Ambiguity, Contradiction, Missing case, Not testable or Scope.
- **Finding:** quote the exact text of the requirement.
- **Decision** stays empty until the product owner or the team answers.

## Rules

- Do not decide alone what the product should do. Turn every doubt into a question.
- Every finding points to a requirement ID and explains why it matters.
- Record every decision, and update the requirements and the out-of-scope list with it.
- Do not invent problems to fill the table.
