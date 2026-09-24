# VoxFlux — Creator current context

**Saved:** 2026-09-24 post-Critic-READY / post-merge  
**Role:** Creator / Творец  
**Language:** Russian  
**Context repository:** `alekseevvb/Context`  
**Context branch:** `main-way`  
**Context root:** `VoxFlux/Creator/`

## 1. Project identity

Primary code repository:

```text
alekseevvb/VoxFluxSTT
```

Controlling branch:

```text
Genesis
```

Current Genesis after accepted P0.0 Candidate.2 fast-forward:

```text
commit:
698d033b0bac1161ed393958ee1e97ca9f70f829

tree:
c60ff9bea5fcd386a27a85ded41562985dd522a8

merge mode:
non-force fast-forward from bdc0c5683816fdcd45e696e24feba1e6612412dc
```

Current feature branch:

```text
phase-0-path-contract
```

Current P0.0 Candidate.2:

```text
commit:
698d033b0bac1161ed393958ee1e97ca9f70f829

tree:
c60ff9bea5fcd386a27a85ded41562985dd522a8

message:
fix(context): preserve tree alignment across directory renames
```

**Critic verdict:** READY for Candidate.2. Merge guard is lifted and Candidate.2 has been fast-forwarded into Genesis. Current guard: P0.1 remains forbidden until P0-47 hash-verified Drive sync and P0.0 smoke PASS.

---

## 2. Roadmap and UAT

Canonical roadmap on Google Drive:

```text
Applications/VoxFluxSTT-IMPROVEMENT-ROADMAP.md
Drive file ID:
1jmIdwnlJe-gueqzYK8DNMeNvwyotCITK

URL:
https://drive.google.com/file/d/1jmIdwnlJe-gueqzYK8DNMeNvwyotCITK/view?usp=drivesdk
```

Roadmap version:

```text
v0.5
status: ACTIVE
last roadmap verdict: READY on v0.4
```

Important governance added in v0.5:

```text
Phase closure =
Critic READY
AND
User UAT PASS
```

Every UAT FAIL must become a numbered finding:

```text
UAT-<PHASE>-NNN
```

Canonical Phase-0 UAT:

```text
Applications/VoxFlux/UAT/UAT-Phase0.md
Drive file ID:
1_KyZt4eDff78-mpH9TxtZuuVa3YAhhIn

URL:
https://drive.google.com/file/d/1_KyZt4eDff78-mpH9TxtZuuVa3YAhhIn/view?usp=drivesdk
```

UAT policy:
- P0.0: only short behavior-preserving smoke after Critic READY + P0-47 hash-verified sync.
- Full Phase-0 UAT: only after P0.1 + P0.2 + P0.3.
- A/B/C have explicit `*-GATE-UAT`.

---

## 3. Accepted characterization / PATH evidence

PATH-01 r4:

```text
READY
baseline commit:
bdc0c5683816fdcd45e696e24feba1e6612412dc

ZIP SHA-256:
c7be82b7d2fe7efe29ab9c8c6db4ce0b901139287156ef18b5f56312ab90456d
```

Accepted r4 identities:

```text
raw_scan:
f46fa4de0e4833d1f50bfc174e87029c98491c165e7103ef0982c654330d8f9f

path_map:
64ead30a0527749ee5c53f50d3de38db1c45e0692a5b259328ca8cebe740d049

exclusions:
d229dbf69bf34e4466a3f138687b36741197159deaee0eff5fa690fd31523700
```

PATH-16 r2:

```text
READY
ZIP SHA-256:
f50e9154327553b31610c7ca15a2b2b4ef3b46caccb6b41933644f26ae6fdb3a
```

Scanner contract:
- exact logical-line text;
- CRLF/CR -> LF;
- split by `"\n"`;
- leading/trailing whitespace preserved;
- unsupported exotic logical-line separators rejected;
- permanent PATH-15 scanner is executed by pytest with file+regex allowlist, not line-number bindings.

---

## 4. P0.0 architecture

Two separate path owners are ratified.

### Deployment side

Module:

```text
Colab/Infrastructure/Libraries/VoxFlux/core/paths.py
```

Contains:

```text
DeploymentPaths
DirectoryManager
```

Rules:
- explicit deployment root only;
- no Colab auto-detection inside library;
- no `.resolve()` in DirectoryManager root handling;
- `from_env()` uses `VOXFLUX_DEPLOYMENT_ROOT`;
- `get_path()` has no side effects;
- `ensure_dirs(*items)` creates only explicit dirs;
- no dynamic SRT/bridge/context helpers in P0.0;
- `default_paths_config()` only creates defaults for existing `PathsConfig`;
- `PathsConfig` remains runtime source of truth until A.1.

### Repository side

Module:

```text
Infrastructure/Runtime/Pathing/repository_paths.py
```

Contains:

```text
RepositoryPaths
RepositoryLayout
```

Rules:
- repository tooling may discover repository root by `pyproject.toml`;
- optional env: `VOXFLUX_REPOSITORY_ROOT`;
- dependency direction only repository -> deployment;
- VoxFlux runtime library does not know repository layout.

Canonical rename in P0.0:
- `Parackeet -> Parakeet`
- `Auxilary -> Auxiliary`

P0.1 remains responsible for:
- `Models/whisper -> Models/Whisper`;
- removing global `XDG_CACHE_HOME` model-cache usage;
- cleaning `Models/pip`;
- updating the old XDG wording in generated context prose.

---

## 5. Candidate.1 history

Candidate.1:

```text
commit:
08573317dd8a39c45cb0fea6ed1df5b7ec730d60

tree:
4a2e86f9702ed9e44ab14eade3d9b4499231d060
```

Critic verdict:

```text
NOT READY
```

Blocking defect:
- notebook cell 2 contained literal characters `\n` instead of real newlines;
- therefore the cell did not compile;
- downstream `directory_manager` references would fail.

Candidate.1 is historical and superseded. It must never be merged.

Critic also requested:
- compile every code notebook cell with `ast.parse`;
- executable PATH-09 for notebook cells 2/4/6;
- restore real `int(...)` conversion in docs tooling instead of typing `cast`;
- permanent Python 3.10 + 3.12 CI matrix;
- remove/justify second `sys.path` bootstrap in `manage_context.py`;
- make PATH-15 actually execute;
- annotate `RepositoryLayout.deployment()`;
- disclose Black check relaxation.

---

## 6. Candidate.2 implementation

Candidate.2 exact identity:

```text
commit:
698d033b0bac1161ed393958ee1e97ca9f70f829

tree:
c60ff9bea5fcd386a27a85ded41562985dd522a8
```

It addresses Candidate.1 findings:
1. notebook cell 2 stores real line breaks;
2. every code cell gets AST compilation coverage;
3. executable PATH-09 runs notebook cells 2/4/6 in a controlled stub environment;
4. docs navigation conversion uses `int(str(meta["order"]))`;
5. CI matrix runs Python 3.10 and 3.12;
6. `manage_context.py` second `sys.path` bootstrap removed;
7. Makefile calls context manager with `python -m Infrastructure.Runtime.Context.manage_context`;
8. `RepositoryLayout.deployment()` has a return type;
9. PATH-15 scanner is executed by pytest with file+regex allowlist;
10. Black relaxation is explicitly disclosed in CHANGELOG and is not called a Black PASS;
11. generated CONTEXT tree alignment was corrected without changing intended semantics.

Controlling CI run:

```text
35949861745
```

Results:

```text
Python 3.10:
pytest 56/56 PASS
Ruff PASS
mypy 0 issues / 23 source files
repository SHA256 PASS

Python 3.12:
pytest 56/56 PASS
Ruff PASS
mypy 0 issues / 23 source files
repository SHA256 PASS

formal-review-package:
PASS
```

Formal Candidate.2 review package SHA-256:

```text
ec9f56be97b0fd2d916926469ea99390f31344373fb2fe7e8676128a5c6591ee
```

GitHub Actions artifact containing formal package SHA-256:

```text
0ad74c751328ddcc46a7ff54c19ee67b2c0acb2c73415ff9e0723a5515cdf234
```

---

## 7. Candidate.2 Critic review surface

Drive split-review folder:

```text
https://drive.google.com/drive/folders/1IzW8ADPhxvM7riq8RjYf3JxgyXuvfxKo
```

Path:

```text
Applications/VoxFluxSTT-Evidence/P0.0/Candidate-02/
698d033b0bac1161ed393958ee1e97ca9f70f829/split-review/
```

Critic verdict file:
- `CRITIC-VERDICT-P0.0-Candidate.2.md`
- Drive ID: `1tiLW6otGyD8UDVk6v9hAMobkSDa06ELO`
- verdict: `READY`
- findings: `P0-F-001`, `P0-F-002` non-blocking; close in P0.1 test-only change-set.

Important files:
- `candidate1-to-candidate2.patch`
- `candidate-core.patch`
- `candidate-tests.patch`
- `ci-workflow.patch`
- `notebook-cells-before-after.txt`
- `notebook-compile.txt`
- `MANIFEST.json`
- `REVIEW.md`
- `CHANGELOG.md`
- `FIXTURE-IDENTITIES.txt`
- `identity.txt`
- `INDEX.md`
- `PROVENANCE.txt`
- `SHA256SUMS`
- `evidence/py310/`
- `evidence/py312/`
- `evidence/behavior/`

Split-review SHA256SUMS file SHA-256:

```text
66dc8650b8092c96018b8bf27e97aab573112481277f3b03bfced9aa4929017e
```

Drive readback status:

```text
DRIVE_SPLIT_SHA256SUMS_BYTE_IDENTICAL
```

---

## 8. Current roadmap frontier

Current completed next actions:
- NEXT-001 through NEXT-006 complete.
- Candidate.2 received Critic READY.
- Genesis fast-forwarded to exact accepted Candidate.2 commit `698d033b0bac1161ed393958ee1e97ca9f70f829`.

Current pending:

```text
NEXT-007 OPEN:
perform P0-47 hash-verified exact accepted Candidate.2/Genesis -> Drive sync,
including Drive rename Parackeet -> Parakeet and Auxilary -> Auxiliary,
without touching Input, Output, or model weights;
then run short P0.0 smoke using UAT-Phase0.md.
Only after smoke PASS move to P0.1.

NEXT-008 BLOCKED:
After P0.1-P0.3:
prepare Phase-0 candidate,
obtain Critic READY,
run full UAT-Phase0.md.
Phase 0 closes only on:
Critic READY AND User UAT PASS.
```

Current UAT status:

```text
P0.0 smoke: NOT RUN
full Phase-0 UAT: NOT RUN
```

---

## 9. Phase-0 UAT corpus plan

Canonical folder:

```text
Applications/VoxFlux/UAT/
```

Planned immutable corpus:
1. `AN-V01-part-001.mp3` — known first-five-minute hallucination case.
2. short clean Russian speech, 1-2 minutes.
3. English fragment `jfk.flac`.
4. one `.mp4` to verify ffmpeg/video path.
5. one intentionally invalid file, e.g. text renamed to `.mp3`.

Before first full UAT:
- record SHA-256 for all corpus files;
- manually listen to first five minutes of `AN-V01-part-001.mp3`;
- explicitly record whether speech exists there;
- this becomes ground truth for P0.2.

Phase-0 UAT checks include:
- fresh Colab runtime, notebook top-to-bottom without manual edits;
- repeated run;
- model weights under `Models/Whisper`;
- no `Models/pip`;
- no unnecessary model re-download;
- corrupt input does not crash entire batch;
- already-ready results are handled according to the implemented policy;
- hallucination suppression on initial non-speech section;
- real speech after ~5:00 remains;
- SRT timing and sequential numbering checked with audio/video player.

---

## 10. Important process rules for the next chat

1. Work as Creator, answer in Russian.
2. Before writes to VoxFluxSTT, inspect exact live branch HEAD.
3. Do not treat historical test counts as current unless rerun/evidence is available.
4. Candidate.1 is NOT READY and historical.
5. Candidate.2 is the only current P0.0 review candidate.
6. Candidate.2 Critic verdict is READY and the exact accepted commit is already merged by fast-forward into Genesis.
7. Current next operation is P0-47 hash-verified Drive deployment sync; P0.1 is still forbidden until P0.0 smoke PASS.
8. After P0-47 sync, run the short P0.0 smoke.
9. Full Phase-0 UAT waits for P0.1-P0.3.
10. A phase closes only with two signatures: Critic READY + User UAT PASS.
11. Keep roadmap and Context checkpoint synchronized with every frontier change.
12. Preserve immutable accepted evidence identities; do not rewrite PATH-01 r4 or PATH-16 r2 packages.
13. Every Critic candidate verdict must be published durably in that candidate's review folder as `CRITIC-VERDICT-<candidate>.md`; chat-only verdicts do not advance Creator state.

---

## 11. Source precedence on recovery

Read in this order:

1. `VoxFlux/Creator/current-context.md`
2. newest `VoxFlux/Creator/checkpoint-*.md`
3. `VoxFlux/Creator/recovery-prompt.md`
4. live `alekseevvb/VoxFluxSTT` branch identities
5. canonical Drive roadmap v0.5
6. canonical UAT document
7. Candidate.2 split-review if Critic review work is being reconciled

If live Git/Drive has advanced after this checkpoint, do **not** roll back. Use this checkpoint as the minimum known state, then reconcile newer durable evidence.

## 12. Immediate next action

Execute NEXT-007:

```text
P0-47 DRIVE SYNC: ALLOWED / NEXT
COLAB SMOKE: ALLOWED AFTER VERIFIED P0-47 SYNC
P0.1 START: FORBIDDEN UNTIL SMOKE PASS
```

Critic READY evidence:
`CRITIC-VERDICT-P0.0-Candidate.2.md`
Drive ID `1tiLW6otGyD8UDVk6v9hAMobkSDa06ELO`.
