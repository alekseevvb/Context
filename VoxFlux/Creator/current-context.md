# VoxFlux — Creator current context

**Saved:** 2026-09-24 post-P0-47 ACCEPTED / historical SRT preserved / smoke ready  
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

**Critic verdict:** READY for Candidate.2. Candidate.2 is fast-forwarded into Genesis. **P0-47 Drive deployment sync is PASS and independently ACCEPTED by the Critic** in `CRITIC-VERDICT-P0-47.md` (Drive ID `1V64e4l-oeJnJZNGJdbaHl-VWqH_nkqVP`). Historical pre-smoke SRT evidence has been copied to UAT and hash-verified. Current guard: P0.1 remains forbidden until P0.0 smoke PASS and durable smoke verdict.

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
NEXT-007 OPEN / PARTIAL:
P0-47 hash-verified exact accepted Candidate.2/Genesis -> Drive sync: PASS.
Drive renames Parackeet -> Parakeet and Auxilary -> Auxiliary: PASS.
17/17 synchronized surfaces raw-byte SHA-256 match exact CI source.
Protected runtime surfaces preserved.
Historical `Output/AN-V01-part-001.srt` preserved as `UAT/AN-V01-part-001.srt` before smoke, SHA-256 `85b357dd5b8d0982357674e42c3e810a4f4b0c8f096e1ebeb3c6e6c887785084`.
P0.0 smoke: NOT RUN / READY TO RUN.
Only after smoke PASS + durable Critic smoke verdict move to P0.1.

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
7. P0-47 hash-verified Drive deployment sync is PASS; P0.1 is still forbidden until P0.0 smoke PASS.
8. Current next operation is the short P0.0 smoke using the published smoke protocol.
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
P0-47 DRIVE SYNC: PASS
COLAB SMOKE: ALLOWED / NEXT
P0.1 START: FORBIDDEN UNTIL SMOKE PASS + durable Critic smoke verdict
```

Critic READY evidence:
`CRITIC-VERDICT-P0.0-Candidate.2.md`
Drive ID `1tiLW6otGyD8UDVk6v9hAMobkSDa06ELO`.


## 13. P0-47 completion evidence

P0-47 Drive sync:

```text
status:
PASS

source commit:
698d033b0bac1161ed393958ee1e97ca9f70f829

source tree:
c60ff9bea5fcd386a27a85ded41562985dd522a8

synchronized surfaces:
17 / 17 SHA-256 exact PASS
```

Evidence folder:

```text
Applications/VoxFluxSTT-Evidence/P0.0/Candidate-02/
698d033b0bac1161ed393958ee1e97ca9f70f829/p0-47-drive-sync/

Drive ID:
19RI5VTsUGSmzSbAXAoPRyfSkS3CLyvrA

SHA256SUMS SHA-256:
ed2f386430e5120940ccfecc81e89ad7ca9f5f136d1c3077abdd89e481864369

Drive readback:
DRIVE_SHA256SUMS_BYTE_IDENTICAL
```

Backup:

```text
Applications/VoxFlux-backup-pre-P0-47-2026-09-24/
Drive ID:
1VxZYYmGgE4olqi44_Y2AlR1CJj_mgTQQ

backup census:
40 files
12 folders
```

Protected runtime surfaces were not synchronized:
- Input/
- Output/
- UAT/
- Models/whisper/
- Models/pip/
- Models/Whisper/

Pre-smoke cache baseline:

```text
Models/whisper/
large-v3.pt
small.pt

medium.pt:
ABSENT
```

First smoke run is expected to download `medium.pt` into `Models/whisper/`.
Second smoke run after runtime restart must reuse it without re-download.

Smoke protocol:

```text
Applications/VoxFluxSTT-Evidence/P0.0/Candidate-02/
698d033b0bac1161ed393958ee1e97ca9f70f829/smoke/SMOKE-PROTOCOL.md

Drive file ID:
1HOkRlEMnfA500v4BZDAR2I3IBIdx3Z1m
```

P0.0 smoke remains `NOT RUN`.


## 14. P0-47 Critic acceptance and smoke Gate 0

Durable Critic verdict:

```text
file:
CRITIC-VERDICT-P0-47.md

Drive ID:
1V64e4l-oeJnJZNGJdbaHl-VWqH_nkqVP

verdict:
ACCEPTED
```

Mandatory smoke precondition added by Critic and completed before Run 1:

```text
source:
Output/AN-V01-part-001.srt

protected copy:
UAT/AN-V01-part-001.srt

protected copy Drive ID:
1WihQVcRn7cpH0Xn09dAfPopMwT1CvvLF

size:
13937 bytes

SHA-256 source:
85b357dd5b8d0982357674e42c3e810a4f4b0c8f096e1ebeb3c6e6c887785084

SHA-256 protected copy:
85b357dd5b8d0982357674e42c3e810a4f4b0c8f096e1ebeb3c6e6c887785084

status:
BYTE_IDENTICAL / PRESERVED
```

Updated smoke protocol:

```text
Applications/VoxFluxSTT-Evidence/P0.0/Candidate-02/
698d033b0bac1161ed393958ee1e97ca9f70f829/
smoke/SMOKE-PROTOCOL.md

Drive file ID:
1HOkRlEMnfA500v4BZDAR2I3IBIdx3Z1m

SHA-256:
3643bc62c6eb48ff8853c90a7896b7b76d7f791a6ed0f903620f5eeade22e487
```

Historical prior protocol retained as:
`SMOKE-PROTOCOL-pre-critic-SRT-precondition.md`
(Drive ID `11hEVlTVrE_4sF3scbF-FFCaYPcP4sfW9`).

Current frontier:

```text
P0-47:
PASS / CRITIC ACCEPTED

SMOKE GATE 0:
PASS

P0.0 COLAB SMOKE:
READY / NOT RUN

P0.1:
BLOCKED
```


## 15. Deferred architecture decision after P0.0 smoke

Durable decision file:

```text
VoxFlux/Creator/decision-2026-09-24-post-smoke-path-module-refactor.md
```

Agreed re-entry rule:

```text
Immediately after:
P0.0 Run 1 PASS
AND
P0.0 Run 2 PASS
AND
durable CRITIC-VERDICT-P0.0-smoke.md

return to the path-module refactor decision before starting broader P0.1 work.
```

Agreed working structure:

```text
core/paths/
├── __init__.py
├── layout.py   -> Layout
├── resolver.py -> Resolver
├── manager.py  -> Manager
└── factory.py  -> Factory
```

Project naming convention:
- one primary class per file;
- prefer one-word file and class names where practical;
- package context carries domain meaning;
- do not modify accepted `698d033...` before smoke closure.


## 16. P0.0 smoke Run 1 and post-smoke runtime requirements

Run 1 durable evidence:

```text
VoxFlux/Creator/evidence/p0-smoke-run1-2026-09-24.md

Drive mirror:
Applications/VoxFluxSTT-Evidence/P0.0/Candidate-02/
698d033b0bac1161ed393958ee1e97ca9f70f829/
smoke/RUN-1-EVIDENCE.md

Drive ID:
10Yl9mipjMByum9Sk4gcUqbpzPw2YqA4K
```

Run 1 result:

```text
RUN_1_PASS
medium.pt PRESENT in Infrastructure/Models/whisper/
fresh SRT PRESENT
unexpected duplicate Parakeet directory: NONE
P0.2 phrase "Продолжение следует": 0 occurrences in entire new SRT
RUN_2_PENDING
```

Post-smoke runtime/output requirements:

```text
VoxFlux/Creator/requirements-2026-09-24-post-smoke-runtime-output.md
```

Captured requirements include:
- per-file media duration;
- per-file wall processing time;
- RTF and x-realtime speed;
- run/model/device timing metadata;
- preserve explicit all-files-in-Input batch behavior;
- run-scoped Output directory named in the direction:
  `YYYY.MM.DD HH-mm. Model - <model and parameters>`;
- machine-readable RUN.json manifest with full parameters and per-file results.

Current accepted code is still unchanged until Run 2 and durable Critic smoke verdict.


## 17. Run 2 partial evidence and console/shutdown requirements

Run 2 partial evidence:

```text
VoxFlux/Creator/evidence/p0-smoke-run2-partial-2026-09-24.md

observed:
processing completed
segments: 129
fresh SRT written

formal no-redownload criterion:
PENDING because Run 2 initialization output was not included
```

Post-smoke runtime/output requirements were extended with:
- R5: automatic successful-run Colab release via runtime adapter and `google.colab.runtime.unassign()`;
- R6: Regenesis-derived Linux-boot-style structured console output;
- R7: strict finalization -> evidence flush -> STOP -> unassign ordering.

Reference console formatter:
`RedHatConsoleFormatter` from Regenesis CPU2 experiment package,
reference formatter SHA-256:
`c12d31fbee23635e239d0f8d6f36211d7e8ab0d633a616881b7de2353cddf893`.

Do not implement these changes before P0.0 smoke closes.


## 18. P0.0 smoke Creator PASS / Critic verdict pending

Run 2 final evidence:

```text
VoxFlux/Creator/evidence/p0-smoke-run2-2026-09-24.md

Drive:
Applications/VoxFluxSTT-Evidence/P0.0/Candidate-02/
698d033b0bac1161ed393958ee1e97ca9f70f829/
smoke/RUN-2-EVIDENCE.md

Drive ID:
1tVUWHUoFH8jQ5sir9oOyCeEhk8PxXldk
```

Creator aggregate smoke result:

```text
VoxFlux/Creator/evidence/p0-smoke-creator-result-2026-09-24.md

Drive:
Applications/VoxFluxSTT-Evidence/P0.0/Candidate-02/
698d033b0bac1161ed393958ee1e97ca9f70f829/
smoke/SMOKE-RESULT.md

Drive ID:
14yfatMN7CcS1ziPffoJFb8W4LtsZbHmO
```

Result:

```text
Run 1:
PASS

Run 2:
PASS

Run 2 no-redownload:
PASS
model initialization line present
1.42G download progress absent
medium.pt Drive object unchanged while later SRT was written

unexpected duplicate Parakeet directory:
NONE

P0.2 preliminary observation after Run 2:
"Продолжение следует" total count = 0
first five minutes = 0

P0.0 SMOKE CREATOR RESULT:
PASS

CRITIC-VERDICT-P0.0-smoke.md:
PENDING
```

P0.1 remains blocked until the durable Critic smoke verdict.

Post-smoke requirements file now also contains R8:
structured lifecycle logging for every executable notebook stage using stable
stage IDs such as BOOT/PATHS/DEPS/DEVICE/MODEL/DISCOVER/RUN/FINALIZE/SHUTDOWN.
Every executable stage must end in PASS or FAIL with timestamp and duration.
This is to be implemented through the same centralized Linux-boot-style
console/logger design recorded in R6, not through independent ad-hoc print calls.


## 19. Durable P0.0 smoke verdict and P0.1 opening

Durable Critic verdict:

```text
file:
CRITIC-VERDICT-P0.0-smoke.md

Drive ID:
1wZ9WrJFpb9vpicyGFflbMCjBJcU-NXcI

verdict:
PASS

genesis:
698d033b0bac1161ed393958ee1e97ca9f70f829

result:
P0.0 CLOSED
P0.1 UNBLOCKED
```

Corrected P0.2 runtime facts from Critic review:
- first five minutes of `AN-V01-part-001.mp3` contain real speech from about 00:30;
- the old large-v3 run replaced real speech with hallucinated text, rather than merely filling silence;
- ground truth for P0.2 must therefore come from listening, not assumed non-speech;
- the new SRT has a gap from approximately 00:03:55.9 to 00:04:25.9 that must be checked against audio;
- identical-input Run 1/Run 2 produced 138 vs 129 segments, so P0.2 criteria must use tolerance-based metrics rather than exact segment counts.

Open non-blocking findings carried into the first P0.1 working branch:
- P0-F-001: narrow PATH-15 allowlist + mutation test;
- P0-F-002: remove dead exclude_prefixes.

Current code working branch:

```text
phase-0-p01-cache-path-cleanup

base:
698d033b0bac1161ed393958ee1e97ca9f70f829

draft PR:
#4

first change-set:
two-class path package + P0-F-001/002
```


## 20. P0.1 Candidate.1 — independent Critic review frontier

P0.0 is durably closed:

```text
CRITIC-VERDICT-P0.0-smoke.md
Drive ID:
1wZ9WrJFpb9vpicyGFflbMCjBJcU-NXcI

verdict:
PASS

result:
P0.0 CLOSED
P0.1 UNBLOCKED
```

Current P0.1 working branch:

```text
phase-0-p01-cache-path-cleanup

candidate commit:
863b104e752a03c80abd54abfa9e8b0220166f9d

candidate tree:
3335b19ab1352f302c6bcc726089ea12fff88a26

base Genesis:
698d033b0bac1161ed393958ee1e97ca9f70f829

draft PR:
#4
https://github.com/alekseevvb/VoxFluxSTT/pull/4
```

Scope:
- two-class path package only: `layout.py -> Layout`, `manager.py -> Manager`;
- compatibility aliases `DeploymentPaths = Layout`, `DirectoryManager = Manager`;
- P0-F-001 closed by narrowed PATH-15 allowlist + mutation test;
- P0-F-002 closed by removal of dead exclusions + live-exclusion regression test;
- global notebook/provider XDG model-cache behavior removed;
- Whisper model directory passed explicitly from `PathsConfig.models_dir` to provider and `whisper.load_model(..., download_root=...)`;
- notebook/context prose use canonical `Infrastructure/Models/Whisper`;
- fail-closed repository migration tool added for `whisper -> neutral -> Whisper` with SHA-256 before/after and rollback.

Verification:

```text
P0 Path Contract Gates run:
36013443813

Python 3.10:
PASS

Python 3.12:
PASS

P0.1 Model Cache Review run:
36013443478

verify-py3.12:
PASS

p01-review-package:
PASS

pytest:
61 passed

Ruff:
PASS

mypy:
Success: no issues found in 26 source files

repository SHA256:
PASS
```

Formal review package:

```text
Drive path:
Applications/VoxFluxSTT-Evidence/P0.1/Candidate-01/
863b104e752a03c80abd54abfa9e8b0220166f9d/review/
P0.1-model-cache-review-package.zip

Drive ID:
18ezwOO9wP9QM32jrQQif_UhveE31ix4A

Drive readback size:
216026 bytes

Drive readback SHA-256:
a8ec96dd89ccd375de11a9fca8ac9240e44b1da9dac31ef47718a223cb1b04ec

GitHub Actions review artifact ID:
10813412251

GitHub Actions outer artifact digest:
c1d1800e3a462f632b7962326027336c3391889e120e5afab620597111766f07

inner review ZIP SHA-256:
a8ec96dd89ccd375de11a9fca8ac9240e44b1da9dac31ef47718a223cb1b04ec
```

Persistent Drive migration status:

```text
Models/whisper weights:
NOT MIGRATED

Models/Whisper:
NOT POPULATED WITH LEGACY WEIGHTS BY P0.1 MIGRATION

Models/pip:
NOT DELETED

pre-migration SHA-256:
NOT YET CAPTURED
```

The Drive connector cannot stream these weight files because even `small.pt` exceeds its 256 MiB download ceiling. Exact SHA-256 must therefore be computed on the mounted Drive by the reviewed migration flow / Colab before mutation.

Current fail-closed guard:

```text
P0.1 CANDIDATE MERGE TO GENESIS:
FORBIDDEN UNTIL INDEPENDENT CRITIC READY

P0.1 DRIVE CODE SYNC:
FORBIDDEN UNTIL INDEPENDENT CRITIC READY

WEIGHT MIGRATION:
FORBIDDEN UNTIL INDEPENDENT CRITIC READY

Models/pip DELETE:
FORBIDDEN UNTIL MIGRATION EVIDENCE IS COMPLETE
```

Next allowed action:
independent Critic review of P0.1 Candidate.1 package.


## 21. P0.1 Candidate.2 — hierarchical Routes review frontier

User-directed path architecture change:

```text
flat Layout strings
->
hierarchical Routes(parent_index, current_index, sign)
```

Current deployment route tree:

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

The full route is derived by parent-chain traversal.

Examples:

```text
Routes.INPUT.path
Input

Routes.LIBRARIES.path
Infrastructure/Libraries

Routes.MODELS_WHISPER.path
Infrastructure/Models/Whisper

Routes.AUXILIARY.path
Infrastructure/Libraries/Auxiliary
```

Responsibility split:

```text
Routes:
logical topology only

Manager:
absolute deployment root + filesystem operations
```

Compatibility:

```python
Layout = Routes
DeploymentPaths = Routes
DirectoryManager = Manager
```

Historical Candidate.1:

```text
commit:
863b104e752a03c80abd54abfa9e8b0220166f9d

PR:
#4 CLOSED

status:
SUPERSEDED / HISTORICAL
```

Current Candidate.2:

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

Verification:

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
Success: no issues found in 26 source files

repository SHA256:
PASS
```

Formal review package:

```text
Drive path:
Applications/VoxFluxSTT-Evidence/P0.1/Candidate-02/
be27c086265d23c43d07e7b160542bad8b18cf32/review/
P0.1-model-cache-review-package.zip

Drive folder ID:
1mdGEytwpO5TQ-iAioeliSEX1yTYSnHC7

Drive file ID:
1e18z1IhUIb6mgR1i23Nds88sltzzS-nr

Drive readback size:
219375 bytes

SHA-256:
0ffbcf7c68a5930f65813d3fe7227e3988c29d1e222f0c46e9b93a18ad80ac0f

GitHub Actions review artifact:
10813664438

GitHub Actions outer digest:
cbd956aa7058f8603f1460387a320bc65278b52c6b9be07cff5696bf2b92ae7b
```

Runtime/deployment state remains unchanged:

```text
Genesis:
698d033b0bac1161ed393958ee1e97ca9f70f829

P0.1 code sync to Drive:
NOT RUN

Models/whisper -> Models/Whisper migration:
NOT RUN

Models/pip deletion:
NOT RUN
```

Fail-closed guard:

```text
Candidate.2 merge to Genesis:
FORBIDDEN UNTIL INDEPENDENT CRITIC READY

Drive code sync:
FORBIDDEN UNTIL INDEPENDENT CRITIC READY

weight migration:
FORBIDDEN UNTIL INDEPENDENT CRITIC READY
```

Next allowed action:
independent Critic review of P0.1 Candidate.2.


## 22. P0.1 Candidate.3 WIP — concise model route names

User-directed exact rename:

```text
Routes.MODELS_WHISPER  -> Routes.WHISPER
Routes.MODELS_PARAKEET -> Routes.PARAKEET
```

Applied across active code, notebook, migration tooling, tests, and changelog.

Permanent regression guard added: active code/tests/notebook must contain zero
occurrences of the legacy identifiers.

Current branch:

```text
phase-0-p01-route-names

HEAD:
b2df6ea04683b0a915c5b007bb8ba7f776f1bd34

draft PR:
#6
https://github.com/alekseevvb/VoxFluxSTT/pull/6
```

Candidate.2 is now superseded for future review because the route API changed.

TOML-backed tree initialization is a separate design item and remains NOT
IMPLEMENTED. First decide the canonical-source/generation direction.

No Drive sync, model migration, Models/pip deletion, or Genesis merge is
authorized from this WIP state.


## 23. Process reset — Drive layout first, then one consolidated P0.1 candidate

Critic feedback accepted.

Historical P0.1 PRs:

```text
PR #4 Candidate.1:
CLOSED / SUPERSEDED

PR #5 Candidate.2:
CLOSED / SUPERSEDED WIP

PR #6 Candidate.3:
CLOSED / SUPERSEDED WIP
```

No current P0.1 review candidate exists.

The useful requirements retained from the WIP branches are:
- final route names: `WHISPER` and `PARAKEET`;
- future Routes model uses parent names, not numeric indexes;
- no compatibility aliases in the final consolidated change-set;
- P0.1 cache fix remains explicit `download_root`;
- model migration and Drive restructuring are one operation.

New order:

```text
1. approve target Drive layout;
2. publish target-layout document + roadmap to Critic through pinned transfer notebook;
3. build one migration notebook with DRY RUN + APPLY;
4. build one consolidated P0.1 candidate;
5. independent Critic review once;
6. migration -> code sync -> smoke, with no notebook run between migration and sync.
```

Target Drive design branch:

```text
branch:
drive-layout-design

source-doc commit:
52db2ef7c1b2c1cb171006083dac16b50f3bbc5d

transfer-notebook commit:
8235c55ea7d6fda93b1b4a9cea4912737e68011e
```

Git documents:

```text
Infrastructure/Documentation/md/DRIVE-TARGET-LAYOUT.md
Infrastructure/Documentation/md/ROADMAP.md
```

Transfer notebook:

```text
Infrastructure/Runtime/Transfer/PUBLISH-DRIVE-LAYOUT-001.ipynb
```

Notebook pins exact source commit:
`52db2ef7c1b2c1cb171006083dac16b50f3bbc5d`.

Authorized Drive write roots for this notebook only:

```text
Applications/VoxFluxSTT/Infrastructure/Environments/AI/Review/
Applications/VoxFluxSTT/Infrastructure/Environments/AI/Roadmap/
```

It has no authorized write path to Input, Output, Models, runtime libraries, UAT or backups.

Next action:
user runs the exact-commit transfer notebook in Colab with a `GITHUB_TOKEN` secret.


## 24. Git -> Colab -> Drive layout publication PASS

User executed:

```text
PUBLISH-DRIVE-LAYOUT-001.ipynb
```

Notebook source commit:

```text
8235c55ea7d6fda93b1b4a9cea4912737e68011e
```

Pinned document source commit:

```text
52db2ef7c1b2c1cb171006083dac16b50f3bbc5d
```

Operator result:

```text
Git commit verified: PASS
Google Drive mount: PASS
DRIVE-TARGET-LAYOUT.md fetched: PASS
ROADMAP.md fetched: PASS
Review materialization: PASS
Roadmap materialization: PASS
MANIFEST.json: WRITTEN
SHA256SUMS: WRITTEN
Drive flush/unmount: PASS
```

Drive review folder:

```text
Applications/VoxFluxSTT/
Infrastructure/Environments/AI/Review/Architecture/Drive-Layout/
52db2ef7c1b2c1cb171006083dac16b50f3bbc5d/

Drive folder ID:
1Oqlt-X5HEFH1oSyHX1Vyi7h6tfZtjH2_
```

Files:

```text
DRIVE-TARGET-LAYOUT.md
Drive ID 1n__EHFGsvHtB-C26bd4RtrINi0PNJcGh
bytes 9091
SHA-256 b63e276cd2a2d686953c63443aee72c4a8daad6ab402c9c3dee0e68c92eb20d8

ROADMAP.md
Drive ID 1FBy_yHGL9iA8ltP_y0U0g-9c1dy5K5Ig
bytes 68633
SHA-256 e0e741431b64c44daa0b4fd576cbe6c031c61e0b78e9f203e1cc8f2bafac744b

MANIFEST.json
Drive ID 1grNvzBFxdBxk6eDNlbvR8MZKPKtI5OnO
bytes 1691
SHA-256 5b4742e1de7a037110b6b49672ccd329a0f96516450e5b580187eb8e26f0a464

SHA256SUMS
Drive ID 1HI0y7AB98NhNwQmuoXdschLdl6Hf7xPK
```

Roadmap live copy:

```text
Applications/VoxFluxSTT/Infrastructure/Environments/AI/Roadmap/

ROADMAP.md
Drive ID 19PciUI-jf5glvIx7Tx6tBSPXtux0LDuJ
bytes 68633

IDENTITY.json
Drive ID 1ZIHlpk8EuGqWfte0ZrJaMf0SZyWi5GOj
bytes 400

IDENTITY source commit:
52db2ef7c1b2c1cb171006083dac16b50f3bbc5d

IDENTITY roadmap SHA-256:
e0e741431b64c44daa0b4fd576cbe6c031c61e0b78e9f203e1cc8f2bafac744b
```

Creator Drive readback verified the actual raw Drive files:

```text
DRIVE-TARGET-LAYOUT.md:
b63e276cd2a2d686953c63443aee72c4a8daad6ab402c9c3dee0e68c92eb20d8 PASS

ROADMAP.md:
e0e741431b64c44daa0b4fd576cbe6c031c61e0b78e9f203e1cc8f2bafac744b PASS

MANIFEST.json:
5b4742e1de7a037110b6b49672ccd329a0f96516450e5b580187eb8e26f0a464 PASS
```

This is the first successful use of the new canonical transfer path:

```text
Git exact commit
-> user-run Colab transfer notebook
-> restricted Drive materialization
-> Creator readback/hash verification
-> Critic handoff
```

No runtime migration, model move, code sync, Input/Output mutation, UAT mutation,
backup mutation, or Models/pip deletion occurred.

Next action:
Critic review of target Drive layout design.


## 25. Drive layout READY_WITH_CONDITIONS integrated; DRY RUN launcher ready

Critic verdict:

```text
file:
CRITIC-VERDICT-DRIVE-TARGET-LAYOUT-52db2ef7.md

Drive ID:
1IS1aPPruLs09XKGOWaq8J8Xhf9AHc2Ge

verdict:
READY_WITH_CONDITIONS
```

C1-C5 were integrated as document amendments with no extra design-review round required.

Amended design commit:

```text
branch:
drive-layout-design

commit:
3c90ccbe4ffe68613bb8af461d25e0b422d31a8f

file:
Infrastructure/Documentation/md/DRIVE-TARGET-LAYOUT.md
```

Integrated rules:
- every current Applications/ top-level object has an explicit fate;
- Drive-ID-referenced runtime/evidence is moved/renamed through Drive API, never copy+delete;
- three write-zone classes are defined: review publication, migration/deployment-sync, move-only identity-preserving data;
- first future APPLY mutation is old-notebook quarantine:
  `VoxFlux.ipynb -> VoxFlux.ipynb.MIGRATED-DO-NOT-RUN`;
- transfer notebooks are centralized under `Environments/AI/Transfer/`.

C5 physical correction already performed:

```text
Transfer folder:
Applications/VoxFluxSTT/Infrastructure/Environments/AI/Transfer/

folder Drive ID:
1L2bgzvZgeYjNmv8P9pUipigOjpSOaLLV

PUBLISH-DRIVE-LAYOUT-001.ipynb:
Drive ID preserved:
1_K6qXiXWgCtkVwVRVB5S-YiDzcFe94J6
```

The publication notebook was moved by Drive API from project root to Transfer; it was not copied.

Canonical DRY RUN notebook:

```text
Git path:
Infrastructure/Runtime/Transfer/DRIVE-MIGRATION-DRY-RUN-001.ipynb

commit:
e5b9ad117da70069a9062bfcefd454c5d2657477

blob:
720fb60612f9ac6d145262525ab6e4273385bcb0
```

It contains no APPLY operations.

DRY RUN behavior:
- exact Git design commit verification;
- Applications/ census by exact Drive IDs;
- legacy runtime/evidence inventory;
- protected SRT identity check;
- Models/pip inventory only;
- SHA-256 of all legacy Whisper weight files;
- SHA-256 of protected historical SRT;
- recursive comparison/hash of both duplicate backups;
- target conflict precheck;
- ID-preserving migration plan;
- writes only DRY RUN evidence under Review/Migration;
- prints `Runtime/evidence mutations: 0` and `APPLY authorized: NO`.

Drive convenience launcher:

```text
Applications/VoxFluxSTT/
Infrastructure/Environments/AI/Transfer/
DRIVE-MIGRATION-DRY-RUN-001.ipynb

Drive ID:
1bTwqe9s85p6MrdeQdRteMa705fTyJtws

bytes:
2789

SHA-256:
d054cc0887c8ef630548bce885f541ae4ac6c2fcbb9fce66454b471e3633753f
```

The launcher verifies and executes exact canonical DRY RUN:

```text
commit:
e5b9ad117da70069a9062bfcefd454c5d2657477

blob:
720fb60612f9ac6d145262525ab6e4273385bcb0
```

Current gate:

```text
allowed:
user runs DRY RUN launcher

forbidden:
APPLY
Drive runtime moves
model relocation
P0.1 code sync
Models/pip deletion
legacy root archival
```

Next action:
user runs the Drive launcher and returns its complete output; Creator then reads split DRY RUN evidence and prepares the Critic handoff.
