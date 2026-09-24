# VoxFlux architecture decision — hierarchical deployment Routes

**Date:** 2026-09-24
**Role:** Creator
**Status:** ACTIVE / IMPLEMENTED_ON_P0.1_CANDIDATE_2

## Accepted baseline

```text
VoxFluxSTT/Genesis:
698d033b0bac1161ed393958ee1e97ca9f70f829

tree:
c60ff9bea5fcd386a27a85ded41562985dd522a8
```

P0.0 is durably closed by Critic PASS.

## User-directed architecture change

The user supplied an earlier project pattern based on an enum tree:

```text
(parent_index, current_index, sign)
```

with:
- stable node indexes;
- explicit parent links;
- one path segment per node;
- full path derived by walking the parent chain;
- a `.path` property and string representation.

VoxFlux adopts the same structural idea for deployment routes.

## Ratified package shape

```text
core/
└── paths/
    ├── __init__.py
    ├── routes.py   -> Routes
    └── manager.py  -> Manager
```

There is no separate concrete `Layout` class.

Compatibility aliases:

```python
Layout = Routes
DeploymentPaths = Routes
DirectoryManager = Manager
```

## Routes tree

```text
RADIX
├── INPUT
├── OUTPUT
└── INFRASTRUCTURE
    ├── MODELS
    │   ├── MODELS_WHISPER
    │   └── MODELS_PARAKEET
    └── LIBRARIES
        └── AUXILIARY
```

Stable tuple definitions:

```text
RADIX            = (0, 0, "")
INPUT            = (0, 1, "Input")
OUTPUT           = (0, 2, "Output")
INFRASTRUCTURE   = (0, 3, "Infrastructure")
MODELS           = (3, 4, "Models")
LIBRARIES        = (3, 5, "Libraries")
MODELS_WHISPER   = (4, 6, "Whisper")
MODELS_PARAKEET  = (4, 7, "Parakeet")
AUXILIARY        = (5, 8, "Auxiliary")
```

## Responsibility boundary

### Routes

Owns only the logical deployment tree:

- `parent_index`
- `current_index`
- `sign`
- `parent`
- `path`
- index lookup
- parent-chain traversal
- cycle/missing-parent detection
- POSIX relative path representation

Example:

```python
Routes.MODELS_WHISPER.path
# PurePosixPath("Infrastructure/Models/Whisper")
```

The path is derived from:

```text
MODELS_WHISPER
-> MODELS
-> INFRASTRUCTURE
-> RADIX
```

No duplicated full string is stored in the enum member.

### Manager

Owns physical filesystem/root behavior:

- explicit absolute deployment root;
- optional root from environment;
- `Routes -> absolute Path`;
- directory creation;
- transitional `PathsConfig` construction.

Example:

```python
manager.get_path(Routes.MODELS_WHISPER)
```

## Deliberate difference from the historical example

The historical enum also performed destructive filesystem operations such as:

```text
create()
remove()
create_structure()
remove_structure()
```

VoxFlux does **not** put these side effects into `Routes`.

Reason:

```text
Routes = declarative tree
Manager = filesystem operations
```

This preserves the useful parent/index/sign model without mixing topology and destructive IO.

## Compatibility and migration

Historical P0.1 Candidate.1:

```text
commit:
863b104e752a03c80abd54abfa9e8b0220166f9d

status:
SUPERSEDED / HISTORICAL
```

Current P0.1 Candidate.2:

```text
branch:
phase-0-p01-route-tree

commit:
be27c086265d23c43d07e7b160542bad8b18cf32

tree:
5596a5ce35e07f469ae984b0ba64e444423aecd5

PR:
#5
```

Candidate.2 preserves all P0.1 model-cache changes from Candidate.1 and changes only the deployment-route representation and its consumers/tests/documentation.

## Verification requirements

Permanent tests must cover:

1. unique `current_index`;
2. valid parent links;
3. expected parent relationships;
4. correct derived relative POSIX paths;
5. compatibility aliases;
6. Manager absolute-path resolution;
7. no regressions in notebook/runtime consumers.

Current Candidate.2 evidence:

```text
pytest:
63 passed

Ruff:
PASS

mypy:
0 issues / 26 source files

Python 3.10:
PASS

Python 3.12:
PASS

repository SHA256:
PASS
```
