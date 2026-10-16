# Framework rules: <framework>

Status: draft (not yet confirmed)
Framework: <name> <version> (from `<lock file>`) · Part: `<root>` · Detected: <YYYY-MM-DD>
Drafted by task-resolver (coding rule 5: framework practices). **Edit freely**: this file belongs to you, and every task follows it once confirmed.

Labels: `repo` (what this codebase already does) · `linter` (configured rule set) · `docs` (official guide for this version) · `inferred` (not checked against a source).

## Architecture and layout

- <rule> — `repo`, e.g. `app/services/billing/charge_service.rb:1`

## Design patterns

| Concern | Pattern used here | Example | Label |
|---|---|---|---|
| Business logic | <service objects / use cases / …> | `<file:line>` | `repo` |
| Data access | <Active Record scopes / repository / …> | `<file:line>` | `repo` |
| Presentation | <presenters / view components / hooks / …> | `<file:line>` | `repo` |
| State | <…> | `<file:line>` | `repo` |
| Background work | <…> | `<file:line>` | `repo` |

## Rules

1. <concrete, checkable instruction> — `<label>`

## Anti-patterns (don't)

- <what this framework and this repo avoid> — `<label>`

## Tests

- <where tests go, factories vs fixtures, what gets mocked> — `<label>`

## Notes

- <repo vs docs disagreements, worth knowing but not to be "fixed" on the side>
