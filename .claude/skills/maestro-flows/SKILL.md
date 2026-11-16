---
name: maestro-flows
description: Use when writing or reviewing Maestro mobile automation flows (YAML). Defines the commons / scenarios / suite structure, how to turn a Gherkin test case into Maestro commands, selectors, waiting, test data and accessibility ID rules.
---

# Maestro flows

Goal: turn approved test cases into Maestro flows that are stable, easy to read and traceable to each test case.

## Structure

```
automation/maestro/
├── commons/      steps reused by many flows (open the app, log in)
├── scenarios/    one folder per app area, one file per scenario
└── suite/        the tests that run: they only chain commons and scenarios with runFlow
```

- Folders are app areas; file names say what the scenario does (`<action>.yaml`), in lowercase with underscores.
- Every file starts with `appId` and `---`: Maestro requires it in every flow file.
- A scenario may cover more than one test case when they are consecutive steps of the same screen.
- Commons take their data as parameters (`${PARAMETER}`). The suite passes the values in the `env` of the `runFlow` that calls the common.

## From test case to flow

Read each test case with the `gherkin-test-cases` skill, then map it:

| Test case | Maestro |
|---|---|
| Given (state, test data) | A common, `launchApp` options or the test data the suite passes |
| When (actions) | `tapOn`, `inputText`, `hideKeyboard`… |
| Then (results) | `extendedWaitUntil` on an element or a text that proves the result |

- Automate only steps that exist in an approved test case. If a step has no test case, stop and report the gap.
- An Examples table: automate the rows the instruction asks for, not all of them by default.
- A Then that cannot be checked reliably stays manual. Say so in the summary.

## Comments

- Commons and scenarios: one line per test case at the top, with its ID and title: `# TC-<AREA>-<NN> · <Title>`. No other comments.
- Suite: a header of up to 3 lines (what the flow is and the test data it needs). No other comments: explanations belong in the README.

## Selectors

- Tap and type by accessibility ID only.
- Use visible text only to confirm a screen or a message, and only when the requirements define that exact text.
- In lists, every card has the same IDs: choose a card by position (`index: 0`) and always check the result afterwards (e.g. a value on the next screen).

## Waiting

- Wait for each screen to appear with `extendedWaitUntil`, never for a fixed time. The timeout is only the maximum wait; it is longer for slow steps (e.g. app start or a payment confirmation).

## Test data and state

- Start from a clean state: `launchApp` with `clearState: true` and `clearKeychain: true` (on iOS, the session can survive in the Keychain).
- Use the test user from the test cases. Never real personal data.

## Style

- Leave a blank line between commands. A tap on a field and the text typed into it (plus `hideKeyboard`) stay together.
- Put text values and IDs in quotes; numbers as text (`"2"`) always need them.

## Accessibility IDs

- Format `<screen>_<element>_<type>`, in lowercase with underscores. Elements shared by several screens use the prefix `nav_`.
- The same ID on iOS (`accessibilityIdentifier`) and Android (`resource-id`).
- The ID never includes the product name or number.
- New IDs are proposed in `docs/accessibility-ids.md` with the status "Proposed — waiting for QA approval". An approved ID is never renamed or removed.

## Templates

A common or scenario:

```yaml
# TC-<AREA>-<NN> · <Test case title>
appId: <app id>
---
- tapOn:
    id: "<screen>_<element>_<type>"
- inputText: ${PARAMETER}

- tapOn:
    id: "<screen>_<element>_<type>"

- extendedWaitUntil:
    visible:
      id: "<screen>_<element>_<type>"
    timeout: <milliseconds>
```

A suite:

```yaml
# <What the flow is>
# Test data: <what the flow needs>
appId: <app id>
---
- runFlow:
    file: ../commons/<common>.yaml

- runFlow:
    file: ../commons/<common with parameters>.yaml
    env:
      PARAMETER: "<value>"

- runFlow:
    file: ../scenarios/<area>/<scenario>.yaml
```
