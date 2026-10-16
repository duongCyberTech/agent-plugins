# Coding rules

Built-in rules for every line of code this skill writes, in every stage that touches code: the plan
designs to them, `apply` builds to them, and `review` checks the change set against them.

| # | Rule | File |
|---|---|---|
| 1 | Stick to SOLID and ACID | [01-solid-acid.md](01-solid-acid.md) |
| 2 | Look for reusable components before writing new ones | [02-reuse-first.md](02-reuse-first.md) |
| 3 | Styling: the CSS library first, nested SCSS | [03-styling.md](03-styling.md) |
| 4 | No fat classes, modules, components or stylesheets | [04-no-fat-units.md](04-no-fat-units.md) |
| 5 | Detect the framework and load its practices into the project rules | [05-framework-practices.md](05-framework-practices.md) |

## Precedence

1. The requirement's own constraints (`Allowed files`, `Git command allowed`) and the repo's
   `workframe/rules/` (including the confirmed `framework-*.md` files from rule 5) come first.
   Where one of them contradicts a rule here, follow it and note the departure in the
   implementation log's *Issues*.
2. Then these rules.
3. Then the project profile's *Conventions* and the nearest sibling code's shape.

These rules shape **new and changed code only**. They never widen the scope: restructuring existing
code the task doesn't otherwise touch, just to satisfy a rule, is a scope change (SKILL.md,
non-negotiable 2). Raise it at the next gate, or list it as *informational* in the code review.

## Where each stage applies them

- **Setup and intake**: draft `workframe/rules/framework-<slug>.md` for each framework that has none
  (rule 5), and have the user confirm it. From then on it is read with the rest of `workframe/rules/`.
- **Plan**: every sub-task's *Related resource files* lists the existing components it reuses
  (rule 2), and the plan says which new units it adds and what each one is for (rules 1 and 4).
- **Implement**: re-read this folder with `workframe/rules/` before the first edit. A departure goes
  in the sub-task log's *Issues*, with the reason.
- **Code review**: each breach in the change set is a finding, reproduced like any other: point at
  the duplicated helper, the hard-coded value a library token covers, or the measured line count.
