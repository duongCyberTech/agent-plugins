# Rule 1: stick to SOLID and ACID

## SOLID: how code is shaped

Apply the spirit in every language, not only class-based ones. In a functional or procedural codebase,
"class" reads as module or function, and "interface" as the function signature or protocol it accepts.

| Principle | In practice |
|---|---|
| **S**ingle responsibility | A unit has one reason to change. Fetching, transforming, persisting and rendering are separate units. If its name needs "and" to describe it, split it. |
| **O**pen/closed | Extend behaviour by adding a unit (a new strategy, handler, variant or config entry), not by growing an `if`/`switch` chain in shared code. |
| **L**iskov substitution | A subtype or implementation honours the contract of what it replaces: same inputs accepted, no stricter preconditions, no surprise exceptions, same return shape. |
| **I**nterface segregation | Callers depend only on what they use. Prefer small, focused interfaces, props and parameter objects over one wide one that most callers half-fill. |
| **D**ependency inversion | High-level logic receives its collaborators (repositories, clients, clocks, mailers) instead of constructing them, so tests can swap them and side effects stay at the edges. |

Don't build abstractions ahead of need: one implementation doesn't need an interface unless the repo
already uses that pattern or a test needs the seam.

## ACID: how data is written

Any change that writes persistent state (a database, a queue, files that must stay consistent) keeps
these properties:

| Property | In practice |
|---|---|
| **A**tomicity | A multi-step write runs in one transaction (or a unit of work): all of it commits, or none of it does. No partial records left behind on an error path. |
| **C**onsistency | Invariants are enforced where the data lives: constraints, foreign keys, unique indexes, validations, not only in the UI. A migration that adds a rule handles the rows that break it. |
| **I**solation | Concurrent callers can't corrupt each other: use row locks, optimistic locking (version columns) or unique constraints for read-modify-write, counters and "check then insert". |
| **D**urability | Report success only after the commit. Side effects outside the transaction (emails, webhooks, jobs) run after commit, or go through an outbox, so a rollback doesn't leave a message for data that doesn't exist. |

When the store or framework can't give one of these (an eventually consistent store, an external API
in the middle of the flow), the plan says so and names the compensation: idempotency keys, retries,
a reconciliation job, or a manual step.
