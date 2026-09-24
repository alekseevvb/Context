# VoxFlux architecture decision — post-P0.0 path module refactor

**Date:** 2026-09-24
**Role:** Creator
**Status:** DEFERRED_UNTIL_P0_0_SMOKE_CLOSE

## Process guard

Do not modify `VoxFluxSTT/Genesis` or accepted P0.0 Candidate.2 before the two-run Colab smoke and durable Critic smoke verdict.

Current accepted code identity:

```text
Genesis:
698d033b0bac1161ed393958ee1e97ca9f70f829

tree:
c60ff9bea5fcd386a27a85ded41562985dd522a8
```

This decision is the first architecture item to return to immediately after smoke closure.

## Agreed design principles

1. Project convention: **one primary class per file**.
2. Prefer **single-word class names and single-word file names** where practical.
3. Package/module context should carry domain meaning instead of repeating it in every class name.
4. Existing P0.0 `core/paths.py` is transitional and violates the convention because it contains both `DeploymentPaths` and `DirectoryManager`.
5. The current path responsibilities should be split into a dedicated `paths` package.
6. Do not collapse distinct responsibilities merely to satisfy short names.

## Working path-module shape

```text
core/
└── paths/
    ├── __init__.py
    ├── layout.py
    ├── resolver.py
    ├── manager.py
    └── factory.py
```

Working class names:

```text
layout.py   -> Layout
resolver.py -> Resolver
manager.py  -> Manager
factory.py  -> Factory
```

Responsibilities:

```text
Layout
- canonical logical deployment structure only
- no filesystem mutation
- no runtime config construction

Resolver
- owns deployment root
- optional from_env()
- logical path -> absolute path
- no directory creation

Manager
- filesystem directory operations only
- ensure/exists and related directory lifecycle operations
- no runtime config construction

Factory
- creates runtime PathsConfig from resolved deployment paths
- owns what is currently default_paths_config()
```

## Naming constraint

Avoid names such as:

```text
DeploymentPathResolver
DeploymentDirectoryManager
PathsConfigFactory
```

when package context already makes the domain clear.

Preferred public API direction:

```python
from VoxFlux.core.paths import Layout, Resolver, Manager, Factory
```

## Additional follow-up noted

The same one-class-per-file convention should later be checked across other modules, including current multi-class files such as `config.py` and `models.py`. This is not authorized to expand the immediate post-smoke path refactor automatically; scope must be decided explicitly before implementation.

## Re-entry condition

Return to this decision immediately after:

```text
P0.0 Colab Run 1 PASS
AND
P0.0 Colab Run 2 PASS
AND
durable CRITIC-VERDICT-P0.0-smoke.md
```

Until then:

```text
ARCHITECTURE WRITE: DEFERRED
P0.1 START: BLOCKED
```
