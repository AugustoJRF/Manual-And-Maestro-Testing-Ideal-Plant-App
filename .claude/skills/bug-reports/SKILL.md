---
name: bug-reports
description: Use when reporting a bug found while testing. Defines the bug report template (severity, priority, version, environment, steps, expected and actual results, evidence, impact) and how to link it to a test case.
---

# Bug reports

Goal: report each bug so anyone can reproduce it without asking questions. Describe what happens, not why: the root cause is analysed by the team.

## Template

```
## BUG-<NN> · <Where, what happens and in which condition>

- **Severity:** Critical | High | Medium | Low
- **Priority:** Critical | High | Medium | Low
- **Version:** <app version>
- **Environment:** <iOS / Android / web> · <test environment>

**Description:**
<One or two sentences: what happens and what should happen.>

**Preconditions**
- <User, data, state>

**Steps to reproduce**
1. <One action per step>

**Expected result:**
<What the requirement says.>

**Actual result:**
<What happens, with the exact values and texts.>

**Evidence:**
<Screenshot, video or GIF, with the problem marked.>

**Impact:**
<What it means for users and the business.>

**How it is caught:**
<The test case that finds it.>
```

## Rules

- One bug per report.
- **Title:** specific enough to understand the bug without opening it.
- **Severity** is how much the bug affects the app and its users. **Priority** is how urgently it must be fixed, based on the business needs. They can be different, e.g. High severity and Critical priority when a popular payment method is blocked.
- **Steps** are actions only, numbered, with the exact data used.
- **Expected result** comes from the requirement, not from opinion.
- **Actual result** has exact values: "the cart contains 11", not "the cart is wrong".
- **Evidence** is always attached. For APIs, include the request that failed, its status code and the error in the response.
- No guesses about the cause. If there is a clue (e.g. the API response is correct, so the problem is in the app), write it as a fact you checked.
- **How it is caught:** name the test case. If none exists, propose one, so the bug enters the regression suite.
