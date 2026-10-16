# Rule 5: detect the framework and load its practices into the project rules

Rules 1–4 hold everywhere. Each framework also has its own idioms: where logic lives, which design
patterns it expects, what its community treats as a mistake. Detect them once per repo, write them
into the project's own rules, and from then on every task reads them with the rest of
`workframe/rules/`.

## When

- At `/task-resolver:setup`, right after the project profile is drafted.
- At intake, when the profile names a framework that has no `workframe/rules/framework-<slug>.md`.
- When a framework's **major** version in the lock file differs from the one recorded in its rules
  file, or a new part (a new app or package in a monorepo) brings a new framework.

## 1. Detect the frameworks and their versions

Start from the project profile's *Stack* table (it comes from `scripts/detect-stack.sh`). Take the
exact version from the lock file (`Gemfile.lock`, `package-lock.json`, `pnpm-lock.yaml`, `yarn.lock`,
`poetry.lock`, `composer.lock`, `go.sum`, …), because practices change between majors: Rails 6 vs 8,
React class components vs hooks, the Next.js pages vs app router, Angular modules vs standalone
components, Vue 2 Options API vs Vue 3 Composition API.

One rules file per framework that shapes how code is written (Rails, Django, Laravel, Spring, React,
Vue, Angular, Next.js, Express, NestJS, Flutter, …). Libraries that don't (a date helper, an HTTP
client) don't get one. The CSS library is covered by [03-styling.md](03-styling.md).

## 2. Find the practices, repo first

Gather them from three sources, in this order of authority:

| Source | What to take | Label |
|---|---|---|
| **The repo itself** | The patterns it already uses, with one or two `file:line` examples each. Read the folder layout and the newest code in each layer: it shows where the code is heading. | `repo` |
| **Its linters** | Framework rule sets already configured (`rubocop-rails`, `eslint-plugin-react-hooks`, `@angular-eslint`, `django-upgrade`, `larastan`, …): they encode practices the team has already agreed to. | `linter` |
| **The framework's official guides**, for the installed major version | Conventions the repo doesn't contradict. Fetch the docs page when a web tool is available and cite the link; otherwise label it *inferred* (SKILL.md, non-negotiable 4). | `docs` / `inferred` |

Where the repo and the docs disagree (the repo uses service objects where the guides would use
model callbacks), **record what the repo does**: it is the convention the next change has to match.
Note the docs' view in one line if it matters, and never "fix" the repo's convention on the side.

Look for these design patterns, layer by layer:

| Layer | Patterns to look for |
|---|---|
| Architecture | MVC, layered, hexagonal / ports and adapters, feature folders, modular monolith, packages in a monorepo |
| Business logic | service objects, interactors / use cases, commands, form objects, domain models, policies |
| Data access | Active Record, repository, data mapper, query objects, scopes, DTOs / serializers |
| Presentation | presenters / decorators, view components, container vs presentational components, custom hooks / composables, templates and partials |
| State | Redux / Zustand / Pinia / NgRx, context, server state (React Query, SWR, Turbo, Hotwire), signals |
| Cross-cutting | dependency injection, middleware, observers / events, background jobs, error handling and result types, i18n, authorization |
| Tests | factories vs fixtures, test pyramid in practice, page objects, what gets mocked |

## 3. Write `workframe/rules/framework-<slug>.md`

Use [the template](../templates/framework-rules.md), one file per framework (`framework-rails.md`,
`framework-react.md`).

- **Short and checkable.** About 10–25 rules, each one a concrete instruction a reviewer can verify
  against a diff ("business logic goes in `app/services/<Domain>/<Verb>Service`, called with `.call`",
  not "keep code clean"). Don't repeat rules 1–4.
- **Each rule carries its label** (`repo`, `linter`, `docs`, `inferred`) and, for `repo`, an example.
- **Anti-patterns** get their own short list: what this framework and this repo avoid.
- Record the framework version and the date it was detected.

## 4. Never overwrite, always confirm

`workframe/rules/` belongs to the user. This skill only **drafts** files there:

- A new file starts with `Status: draft (not yet confirmed)`. Show it to the user: at the end of
  setup, or at G1 under *Needs your say*. On their answer, set `Status: confirmed <date>`, applying
  their edits.
- Until it is confirmed, follow a draft as guidance, but don't hold a change to it where it conflicts
  with what the user says or what the code shows: ask instead.
- An existing file is never rewritten. For a version bump or a new finding, propose the change as an
  addition (or a dated note on the rule it replaces) and write it only on a yes.
- Once confirmed, the file is part of `workframe/rules/` like any other: it outranks rules 1–5
  ([index.md](index.md#precedence)), and plan, apply and review follow it.
