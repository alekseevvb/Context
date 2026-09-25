# VoxFlux Creator checkpoint — P0.1 Candidate.2 Routes awaiting Critic

**Date:** 2026-09-24
**Role:** Creator
**Status:** controlling Creator checkpoint

## Accepted Genesis baseline

```text
Genesis:
698d033b0bac1161ed393958ee1e97ca9f70f829

tree:
c60ff9bea5fcd386a27a85ded41562985dd522a8
```

P0.0 is closed by durable Critic PASS.

## P0.1 Candidate history

Candidate.1:

```text
branch:
phase-0-p01-cache-path-cleanup

commit:
863b104e752a03c80abd54abfa9e8b0220166f9d

status:
SUPERSEDED / HISTORICAL

PR:
#4 CLOSED
```

Candidate.1 remains immutable evidence and must not be merged.

## Current P0.1 Candidate.2

```text
branch:
phase-0-p01-route-tree

commit:
be27c086265d23c43d07e7b160542bad8b18cf32

tree:
5596a5ce35e07f469ae984b0ba64e444423aecd5

base Genesis:
698d033b0bac1161ed393958ee1e97ca9f70f829

draft PR:
#5
https://github.com/alekseevvb/VoxFluxSTT/pull/5
```

## User-directed Routes architecture

The flat path enum is replaced by a hierarchical enum modeled after the user's earlier project:

```python
class Routes(Enum):
    def __init__(self, parent, index, sign):
        self.parent_index = parent
        self.current_index = index
        self.sign = sign
```

Current tree:

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

Current tuples:

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

`Routes.path` is a relative `PurePosixPath` built by walking parents.

Examples:

```text
Routes.INPUT.path
Input

Routes.MODELS.path
Infrastructure/Models

Routes.MODELS_WHISPER.path
Infrastructure/Models/Whisper

Routes.AUXILIARY.path
Infrastructure/Libraries/Auxiliary
```

Compatibility aliases:

```python
Layout = Routes
DeploymentPaths = Routes
DirectoryManager = Manager
```

There is no separate concrete Layout class.

## Responsibility boundary

```text
Routes:
declarative topology only

Manager:
absolute deployment root
route -> absolute filesystem Path
directory creation
transitional PathsConfig construction
```

Historical enum-style destructive methods such as `remove()`, `create()`,
`create_structure()`, and `remove_structure()` are intentionally NOT placed
inside Routes.

## P0.1 model-cache scope preserved

Candidate.2 preserves Candidate.1 P0.1 behavior:

- no notebook/provider global `XDG_CACHE_HOME`;
- canonical Whisper path is `Infrastructure/Models/Whisper`;
- pipeline passes `config.paths.models_dir`;
- provider uses `whisper.load_model(..., download_root=models_dir)`;
- fail-closed SHA-256 cache migration tool remains.

## Verification

```text
P0 Path Contract Gates:
run 36015894350

Python 3.10:
PASS

Python 3.12:
PASS

P0.1 Model Cache Review:
run 36015894097

verify-py3.12:
PASS

p01-review-package:
PASS

pytest:
63 passed

Ruff:
PASS

mypy:
0 issues / 26 source files

repository SHA256:
PASS
```

## Formal review package

```text
Drive folder:
Applications/VoxFluxSTT-Evidence/P0.1/Candidate-02/
be27c086265d23c43d07e7b160542bad8b18cf32/review/

folder ID:
1mdGEytwpO5TQ-iAioeliSEX1yTYSnHC7

file:
P0.1-model-cache-review-package.zip

Drive file ID:
1e18z1IhUIb6mgR1i23Nds88sltzzS-nr

size:
219375 bytes

SHA-256:
0ffbcf7c68a5930f65813d3fe7227e3988c29d1e222f0c46e9b93a18ad80ac0f
```

GitHub Actions:

```text
run:
36015894097

artifact ID:
10813664438

outer artifact digest:
cbd956aa7058f8603f1460387a320bc65278b52c6b9be07cff5696bf2b92ae7b
```

## Runtime Drive state

Unchanged:

```text
P0.1 code sync:
NOT RUN

Models/whisper -> Models/Whisper:
NOT RUN

Models/pip deletion:
NOT RUN
```

## Fail-closed guard

Until durable independent Critic READY on Candidate.2:

```text
MERGE TO GENESIS:
FORBIDDEN

P0.1 DRIVE CODE SYNC:
FORBIDDEN

WEIGHT MIGRATION:
FORBIDDEN

Models/pip DELETE:
FORBIDDEN
```

## Next allowed action

Independent Critic review of P0.1 Candidate.2:

```text
Drive file ID:
1e18z1IhUIb6mgR1i23Nds88sltzzS-nr
```
