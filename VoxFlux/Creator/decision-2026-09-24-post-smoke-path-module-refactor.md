# VoxFlux architecture decision — post-P0.0 path module refactor

**Date:** 2026-09-24
**Role:** Creator
**Status:** ACTIVE / IMPLEMENTED_ON_P0.1_WORKING_BRANCH

## Controlling code identity

Accepted P0.0 / Genesis baseline:

```text
Genesis:
698d033b0bac1161ed393958ee1e97ca9f70f829

tree:
c60ff9bea5fcd386a27a85ded41562985dd522a8
```

P0.0 smoke is closed with durable Critic PASS. P0.1 is unblocked.

## Agreed design principles

1. Project convention: **one primary class per file**.
2. Prefer **single-word class names and single-word file names** where practical.
3. Package/module context carries domain meaning instead of repeating it in class names.
4. The accepted P0.0 `core/paths.py` was transitional because it contained two classes.
5. Do not introduce `Resolver` or `Factory` without a concrete responsibility that cannot be owned cleanly by the current two concepts.
6. Preserve accepted P0.0 public names through compatibility aliases during the Phase-0 transition.

## Ratified path-module shape

```text
core/
└── paths/
    ├── __init__.py
    ├── layout.py
    └── manager.py
```

Classes:

```text
layout.py  -> Layout
manager.py -> Manager
```

Responsibilities:

```text
Layout
- canonical logical deployment structure only
- Enum values only
- no filesystem mutation
- no runtime config construction

Manager
- owns explicit deployment root
- optional from_env()
- logical Layout item -> absolute path
- explicit directory creation through ensure_dirs()
- creates the existing PathsConfig defaults while that transitional API remains in Phase 0
```

Compatibility aliases in `paths/__init__.py`:

```text
DeploymentPaths = Layout
DirectoryManager = Manager
```

These aliases are not additional classes and preserve the accepted P0.0 import surface.

## Explicitly rejected for the current scope

```text
resolver.py -> Resolver
factory.py  -> Factory
```

They are not introduced because the present code has no independent responsibility requiring them. Adding them now would be speculative abstraction and would violate the Phase-0 NO-overengineering rule.

## Current implementation

Working branch:

```text
phase-0-p01-cache-path-cleanup
```

The two-file package is implemented there and the dedicated regression gates passed before the P0.1 model-cache changes were layered on top.

The same one-class-per-file convention should later be reviewed across other multi-class modules such as `config.py` and `models.py`, but that is outside this narrow path change-set unless explicitly scoped later.
