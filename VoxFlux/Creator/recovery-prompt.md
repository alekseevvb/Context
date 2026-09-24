# VoxFlux Creator recovery prompt

Восстанови текущий Creator-контекст проекта **VoxFlux / VoxFluxSTT**.

Работай как **Творец (Creator)** и отвечай только по-русски.

Контекст:

```text
repo:
alekseevvb/Context

branch:
main-way

root:
VoxFlux/Creator/
```

Сначала полностью прочитай строго по порядку:

```text
1. VoxFlux/Creator/current-context.md
2. VoxFlux/Creator/checkpoint-2026-09-24-p01-candidate2-routes-awaiting-critic.md
3. VoxFlux/Creator/decision-2026-09-24-post-smoke-path-module-refactor.md
4. VoxFlux/Creator/requirements-2026-09-24-post-smoke-runtime-output.md
5. VoxFlux/Creator/recovery-prompt.md
```

Затем обязательно сверь live Git state:

```text
code repo:
alekseevvb/VoxFluxSTT

Genesis expected minimum:
698d033b0bac1161ed393958ee1e97ca9f70f829

current P0.1 branch:
phase-0-p01-route-tree

P0.1 Candidate.2 expected:
be27c086265d23c43d07e7b160542bad8b18cf32

tree:
5596a5ce35e07f469ae984b0ba64e444423aecd5

draft PR:
https://github.com/alekseevvb/VoxFluxSTT/pull/5
```

Historical P0.1 Candidate.1:

```text
commit:
863b104e752a03c80abd54abfa9e8b0220166f9d

PR:
#4 CLOSED

status:
SUPERSEDED / HISTORICAL
```

Do not merge Candidate.1.

P0.0 state:

```text
CLOSED

durable verdict:
CRITIC-VERDICT-P0.0-smoke.md

Drive ID:
1wZ9WrJFpb9vpicyGFflbMCjBJcU-NXcI

verdict:
PASS
```

Current path architecture:

```text
core/paths/
├── __init__.py
├── routes.py   -> Routes
└── manager.py  -> Manager
```

Routes uses:

```text
(parent_index, current_index, sign)
```

Tree:

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

Compatibility aliases:

```python
Layout = Routes
DeploymentPaths = Routes
DirectoryManager = Manager
```

Do not recreate a separate concrete Layout class unless explicitly reauthorized.

Candidate.2 verification:

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

Formal review package:

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

Current Critic verdict for P0.1 Candidate.2:

```text
PENDING
```

До durable Critic READY строго запрещено:

```text
- merge Candidate.2 to Genesis;
- P0.1 Drive code sync;
- Models/whisper -> Models/Whisper weight migration;
- Models/pip deletion.
```

Drive runtime remains pre-migration.

If live Context/Genesis/P0.1 branch advanced, do not roll back. Treat this checkpoint as minimum known state and reconcile all newer durable evidence first.

После восстановления кратко выведи:

```text
Роль
Context HEAD
Genesis HEAD
P0.1 branch HEAD
P0.1 Candidate identity
Routes architecture
current Critic verdict
Drive migration state
следующее разрешённое действие
```
