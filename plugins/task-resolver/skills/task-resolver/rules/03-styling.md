# Rule 3: styling

## Detect the CSS library first

Before writing any style, find out which CSS library the project uses, and record it in the project
profile under *Conventions → Styling* so later tasks don't detect it again.

| Look in | For |
|---|---|
| Package manifest (`package.json`, `Gemfile`, `composer.json`, `pyproject.toml`, …) | `tailwindcss`, `bootstrap`, `bulma`, `@mui/material`, `@chakra-ui/react`, `antd`, `@mantine/core`, `vuetify`, `primevue`, `tailwindcss-rails`, `bootstrap` gem, … |
| Config files | `tailwind.config.*`, a `@theme` block or `@import "tailwindcss"` (Tailwind v4), `postcss.config.*`, a theme file (`theme.ts`, `_variables.scss`) |
| Entry stylesheets | `@tailwind`, `@import "bootstrap"`, `@use "bulma"` and similar |
| Layout templates | `<link>` tags loading a library from a CDN |
| Existing markup | the class names already in use (`flex gap-4 text-sm`, `d-flex mb-3 btn btn-primary`) |

## The library has the highest priority

If a style can be expressed with the library's utilities, components or design tokens, use them, and
**don't add new CSS or SCSS** for it. That covers spacing, colour, typography, layout, borders,
shadows, breakpoints and states.

- Use the project's configured theme values (`bg-primary`, `text-muted`, `p-4`, `theme.palette.…`),
  not raw values that happen to match.
- Prefer the library's component (`btn`, `card`, `<Button>`) over a hand-built look-alike.
- Arbitrary values (`w-[317px]`, inline `style=`) are a last resort. If a value repeats, it belongs in
  the theme config, not copied across files.

Write custom CSS or SCSS only for what the library can't express (a complex animation, a third-party
widget override, a one-off layout the utilities can't reach). Even then, build it from the library's
tokens and the repo's existing variables and mixins; never redefine a colour or spacing value the
theme already has.

No CSS library in the project: follow the repo's existing styling approach (CSS modules,
styled-components, plain CSS or SCSS with its variables) and reuse its tokens. Never introduce a new
CSS library as a side effect of a task; that is a decision for a gate.

## SCSS: nested format

When the project already uses SCSS (`.scss` files, a `sass` dependency), write it nested, following the
markup's structure:

```scss
.order-card {
  padding: $space-4;

  &__header {
    display: flex;
    gap: $space-2;
  }

  &__title {
    font-weight: $font-weight-bold;

    &:hover {
      color: $color-primary;
    }
  }

  &--highlighted {
    border-color: $color-accent;
  }

  @include media-breakpoint-up(md) {
    padding: $space-6;
  }
}
```

- Nest children, `&` modifiers, pseudo-classes and media queries under their block, not as flat
  top-level selectors repeating the parent.
- Keep nesting **3 levels deep at most**. Deeper nesting produces selectors that are hard to
  override; split into a new block instead ([04-no-fat-units.md](04-no-fat-units.md)).
- Follow the repo's naming scheme (BEM, as above, or whatever the existing files use).
- Don't use SCSS in a project that doesn't already; that's a new dependency.
