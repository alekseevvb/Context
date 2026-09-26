# VoxFlux Critic Context

Critic context snapshots are stored canonically under:

```text
VoxFlux/Critic/Context/
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

`Context/` contains independent Critic state snapshots only.

Live recovery instructions remain outside `Context/`:

```text
VoxFlux/Critic/recovery-prompt.md
```

## Current latest snapshot

```text
VoxFlux/Critic/Context/2026.26.09/01. 17-28-00. P0.4 Owner-run ready Git-direct UAT/CONTEXT.md
```
