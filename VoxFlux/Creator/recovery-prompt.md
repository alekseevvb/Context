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
2. VoxFlux/Creator/checkpoint-2026-09-24-p01-candidate1-awaiting-critic.md
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

P0.1 working branch:
phase-0-p01-cache-path-cleanup

P0.1 Candidate.1 expected:
863b104e752a03c80abd54abfa9e8b0220166f9d

tree:
3335b19ab1352f302c6bcc726089ea12fff88a26

draft PR:
https://github.com/alekseevvb/VoxFluxSTT/pull/4
```

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

P0.1 Candidate.1 verification:

```text
P0 Path Contract Gates:
run 36013443813
Python 3.10 PASS
Python 3.12 PASS

P0.1 Model Cache Review:
run 36013443478
verify-py3.12 PASS
p01-review-package PASS

pytest:
61 passed

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
Applications/VoxFluxSTT-Evidence/P0.1/Candidate-01/
863b104e752a03c80abd54abfa9e8b0220166f9d/review/

folder ID:
1mhBcFqSpiNj3iDvjs3yNbDzXXICa35PZ

file:
P0.1-model-cache-review-package.zip

Drive file ID:
18ezwOO9wP9QM32jrQQif_UhveE31ix4A

size:
216026 bytes

SHA-256:
a8ec96dd89ccd375de11a9fca8ac9240e44b1da9dac31ef47718a223cb1b04ec
```

Current Critic verdict for P0.1 Candidate.1:

```text
PENDING
```

До durable Critic READY строго запрещено:

```text
- merge P0.1 candidate to Genesis;
- Drive code sync for P0.1;
- migration of Models/whisper weights;
- deletion of Models/pip.
```

Current Drive model state remains pre-migration:

```text
Models/whisper/
- large-v3.pt
- small.pt
- medium.pt

Models/Whisper/
- canonical target directory exists

Models/pip/
- still exists and is confirmed pip cache
```

Exact SHA-256 of the large Drive weights has NOT yet been captured because the connector raw-download ceiling is 256 MiB. This must be computed on mounted Drive immediately before mutation using the reviewed migration flow.

Path architecture ratified after P0.0:

```text
core/paths/
├── __init__.py
├── layout.py  -> Layout
└── manager.py -> Manager

compatibility aliases:
DeploymentPaths = Layout
DirectoryManager = Manager
```

Do not reintroduce speculative `Resolver` or `Factory` classes.

Deferred runtime/output work (Linux-style formatter, timing/RTF, RUN.json, run-scoped output folders, automatic Colab shutdown, executable-stage lifecycle logging) is NOT part of Phase 0. It remains recorded in:
`requirements-2026-09-24-post-smoke-runtime-output.md`
and must later be assigned explicit roadmap items under the appropriate later phase.

If live Context/Genesis/P0.1 branch advanced, do not roll back. Treat this checkpoint as minimum known state and reconcile all newer durable evidence first.

После восстановления кратко выведи:

```text
Роль
Context HEAD
Genesis HEAD
P0.1 branch HEAD
P0.1 Candidate identity
current Critic verdict
Drive migration state
следующее разрешённое действие
```
