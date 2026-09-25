# VoxFlux Creator Context

Creator context snapshots are stored canonically under:

```text
VoxFlux/Creator/Context/
└── YYYY.DD.MM/
    └── NN. HH-mm-ss. Topic/
        └── CONTEXT.md
```

## Naming contract

- `YYYY.DD.MM` is the project-local calendar date in **Asia/Jerusalem**.
- `NN` is a two-digit sequence within that date: `01`, `02`, ...
- `HH-mm-ss` is the project-local snapshot time.
- `Topic` is a short human-readable description of the saved frontier.
- Every snapshot file is named exactly `CONTEXT.md`.

## Immutability

Once a snapshot is committed, it is immutable.

A later save must create a new numbered directory. Do not rewrite an older snapshot to represent a newer state.

## What belongs here

`Context/` contains Creator state snapshots only.

These remain outside `Context/`:

```text
VoxFlux/Creator/recovery-prompt.md
VoxFlux/Creator/decision-*.md
VoxFlux/Creator/requirements-*.md
VoxFlux/Creator/evidence/
```

They are live recovery instructions or durable supporting material, not context snapshots.

## Current latest snapshot

At the time this convention was introduced:

```text
VoxFlux/Creator/Context/2026.25.09/03. 04-06-03. Candidate1 expanded review context save/CONTEXT.md
```

Historical flat `checkpoint-*.md` snapshots and `current-context.md` were migrated without byte changes in the reorganization commit, then the latest snapshot and recovery instructions were updated in the follow-up commit.
