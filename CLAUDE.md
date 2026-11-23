# Ideal Plant — QA project

QA project for **Ideal Plant**, a concept plant shop mobile app (iOS and Android). The screens are mockups and there is no app build yet.

## Where things are

| Path | Content |
|---|---|
| `docs/requirements.md` | Requirements per screen, out-of-scope items and test data |
| `docs/test-cases.md` | Manual test cases (Given / When / Then) |
| `docs/accessibility-ids.md` | Accessibility IDs the app must provide for automation |
| `docs/bug-report-examples.md` | Example bug reports |
| `docs/screens/` | App screens and animations |
| `automation/maestro/` | Maestro flows: `commons/`, `scenarios/`, `suite/` |
| `automation/reports/` | Review reports, with the QA decision on each finding |
| `.claude/skills/` | Project rules as skills |
| `.claude/agents/` | Automation and reviewer agents |
| `.mcp.json` | Maestro MCP server, for when a build exists |

## Workflow

1. Review new requirements with the `requirements-analysis` skill.
2. Write test cases with the `gherkin-test-cases` skill.
3. Automate approved test cases with the `maestro-automation-agent`.
4. Review every automation round with the `automation-reviewer-agent`.
5. Report bugs with the `bug-reports` skill.

## General rules

- Everything is written in English.
- All test data is fictional. Never use real personal data.
- Every test case links to its requirements, and every flow to its test cases.
- Nothing is final until the QA approves it. Turn doubts into questions instead of assuming.
- Use the Maestro MCP tools only when a build is running on a device or emulator.
