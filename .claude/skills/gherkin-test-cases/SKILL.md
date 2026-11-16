---
name: gherkin-test-cases
description: Use when writing, reviewing or reading manual test cases. Defines the Given / When / Then format, the header with requirements, priority and automation candidate, Examples tables and traceability rules.
---

# Gherkin test cases

Goal: write manual test cases in Given / When / Then that anyone can run and get the same result, each one linked to the requirements it covers.

## Format

````
**TC-<AREA>-<NN> · <Scenario name>**<br>
Requirements: <IDs> · Priority: <High | Medium | Low> · Automation candidate: <Yes | No>

```gherkin
Given <the starting state>
And <more of the starting state>
When <the action>
Then <the observable result>
And <another observable result>
```
````

- **ID:** the area prefix of the requirements (e.g. LOG for Login) and a number.
- **Title:** the name of the scenario, based on what is tested.

## Rules

**Steps**
- **Given** sets the state: screen, data, cart. Never actions the test is about.
- **When** contains actions only. A check never goes in When.
- **Then** contains observable results only, and is always last: no action after a Then.
- One behaviour per test case.

**Variations**
- Same steps with different data → a Scenario Outline: `<placeholders>` in the steps and an `Examples:` table.
- Use boundary values and equivalence partitions to choose the rows (e.g. the minimum, a middle value and the maximum).
- A note under the test case (`> ...`) explains a row or a waiting rule when it is not obvious.

**Test data**
- Common preconditions (test user, environment, seed data) are written once at the top of the document, not in each test case.
- Use generic names (item1, item2) when any item from the test data works.
- All data is fictional.

**Coverage**
- Every requirement is covered by at least one test case.
- Include negative cases and the back navigation on both platforms: the Android system back (button or swipe) and the iOS swipe back gesture.

**Priority** (how important it is to run the test, based on risk)
- **High:** the main user flows and the areas with the highest risk, such as login, payments or user data. Run in every regression.
- **Medium:** important features that have a workaround, and checks of content and states.
- **Low:** minor features and visual details. Run when there is time.

**Automation candidate**
- **Yes:** stable, repeatable and valuable in regression.
- **No:** mostly visual, or depends on something outside the app.

## Summary

The document starts with a summary per screen: number of test cases, high priority and automation candidates.

## Template with variations

````
**TC-<AREA>-<NN> · <Scenario name>**<br>
Requirements: <IDs> · Priority: <High | Medium | Low> · Automation candidate: <Yes | No>

```gherkin
Given <the starting state with <parameter 1>>
And the user is on <screen>
When the user <action with <parameter 2>>
Then <element> is <result>

Examples:
  | parameter 1 | parameter 2 | result |
  | <value>     | <value>     | <value> |
  | <value>     | <value>     | <value> |
```

> <Optional note explaining a row or a waiting rule.>
````
