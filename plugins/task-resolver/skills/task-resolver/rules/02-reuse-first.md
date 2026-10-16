# Rule 2: look for reusable components first

Before writing a new feature, function, component, hook, helper, style or test fixture, search the
repo for one that already does the job, or most of it.

## Where to look

- **The nearest sibling feature**: how it fetches, validates, renders, handles errors and is tested.
- **Shared folders**: `components/`, `ui/`, `shared/`, `common/`, `lib/`, `utils/`, `helpers/`,
  `hooks/`, `services/`, `concerns/`, `mixins/`, design-system packages in a monorepo.
- **Installed dependencies**: the package manifest. If a library already in the repo does it (date
  handling, validation, HTTP, formatting, UI primitives), use it rather than hand-rolling it. Adding a
  new dependency is a decision for a gate, not a default.
- **The styling layer**: the CSS library's utilities and tokens, and the repo's own variables and
  mixins ([03-styling.md](03-styling.md)).
- **Tests**: existing factories, fixtures, builders and page objects.

Search by behaviour as well as by name: grep for the key call, the endpoint, the table, or the
user-visible string, not only for a guessed function name.

## What to do with what you find

| Found | Do |
|---|---|
| A unit that does the job | Use it as it is. |
| One that almost does it | Extend it in a backward-compatible way (a new optional parameter, prop or variant), keeping its existing callers and tests green. |
| Two or more near-duplicates | Use the one the newest code uses. Merging them is a scope change unless the requirement asks for it. |
| Nothing | Write a new unit, in the place and shape the nearest sibling would have used, so the next task can find it. |

## Record it

- **Plan**: each sub-task lists what it reuses under *Related resource files* (`file:line`), and
  says why when it writes something new that looks like an existing unit.
- **Implementation log**: name what was reused and what was added.
