# Rule 4: no fat classes, modules, components or stylesheets

Keep every unit small enough to read in one sitting and to test on its own. This is rule 1's single
responsibility, measured.

## Signals that a unit is getting fat

These are prompts to split, not hard limits. The repo's own linter settings (max-lines, complexity,
RuboCop metrics, …) override them where they exist.

| Unit | Split when it |
|---|---|
| Class / module / file | passes ~300 lines, or holds more than one concern (e.g. HTTP handling and business rules and persistence) |
| Function / method | passes ~50 lines, takes more than ~4 parameters, or nests conditionals more than 3 deep |
| UI component | fetches data, manages complex state **and** renders several distinct sections; or its props list keeps growing with flags that switch whole branches of markup |
| Stylesheet (CSS / SCSS) | passes ~200 lines, styles more than one block or component, or nests more than 3 levels |
| Test file | covers several units, or its setup outgrows its assertions |

## How to split

- **Classes and modules**: pull each concern into its own unit (service, repository, validator,
  presenter, policy) and have the original delegate to it.
- **Functions**: extract named steps; the original reads as a short sequence of calls.
- **Components**: separate data loading (a hook, loader or container) from presentation, and break a
  large view into child components, one per section.
- **Stylesheets**: one file per block or component, next to the component when the repo does that;
  shared values go into the existing variables or the library's theme
  ([03-styling.md](03-styling.md)).

Name and place the new units the way the nearest sibling code does ([02-reuse-first.md](02-reuse-first.md)).

## Existing fat code

- **Don't make it fatter.** When a task needs to add to a file that is already past these signals,
  put the new code in a new unit and call it from there.
- **Don't split it on the side.** Breaking up existing code the task doesn't otherwise change is a
  scope change: raise it at the next gate, or list it as *informational* in the code review.
