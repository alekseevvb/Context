# VoxFlux Creator context — Environment PR #14 green / P0.3 probe v004 pending

Saved: 2026-09-25 19:52:56 +03:00  
Role: Creator  
Context repository: `alekseevvb/Context`, branch `main-way`

## 1. Response contract

Work as **Творец (Creator)**.

Answer only in Russian.

Every substantive response must contain:

```text
Роль
Время обработки сообщения
Время ответа
```

Before context pressure becomes high, issue a copyable fenced warning with `⚠️`.

Owner preference:
- code/product first;
- TDD;
- simple readable implementation;
- GRASP/SOLID and GoF only where they simplify responsibilities;
- small coherent PRs;
- no unnecessary process/evidence bureaucracy;
- do not make Owner perform AI/internal infrastructure work that tools can do;
- one obvious RUN-ME when an owner action is genuinely required.

Controlling owner policy in the code repository:

```text
Infrastructure/Documentation/md/VoxFluxSTT-OWNER-WORKING-RULES.md
```

## 2. Code repository live frontier

Repository:

```text
alekseevvb/VoxFluxSTT
```

### Genesis

Live `Genesis`:

```text
HEAD
1ac03afa5a040148a5842bff938a0ef478f70039

tree
16a2dfd63ff80de6a4c7f6e393d01209ad854960

message
docs(context): save session context snapshot after pulling Silero VAD and DriveSync

parent
97e2038c99c759df4ac135bd66ce6a463844a1be
```

The parent `97e2038...` is the merge of PR #13 permanent Drive sync.

### Active Environment branch

```text
branch
phase-0-environment-runtime

HEAD
a3944a0bd3d9e851c250b354c022f1fdaaa03d9a

tree
07d28ab85ad3909db497c3b61f50caba8667c5a8

message
test(environment): assert Auxiliary route at notebook source
```

Current comparison against live Genesis:

```text
ahead_by: 29
behind_by: 1
changed_files: 22
```

The branch was created from the pre-context Genesis frontier, so the single behind commit is the later context-only Genesis commit `1ac03afa...`. Do not reset or recreate the branch blindly. PR is currently mergeable.

## 3. PR #14 — controlling infrastructure change-set

PR:

```text
#14
refactor(runtime): centralize notebook lifecycle in Environment

base:
Genesis / 1ac03afa5a040148a5842bff938a0ef478f70039

head:
phase-0-environment-runtime / a3944a0bd3d9e851c250b354c022f1fdaaa03d9a

state:
OPEN
draft:
false
merged:
false
mergeable:
true

commits:
29

changed files:
22

additions:
801

deletions:
566
```

PR URL:

```text
https://github.com/alekseevvb/VoxFluxSTT/pull/14
```

CI:

```text
run
36162307065

P0 Path Contract Gates
SUCCESS

gates-py3.10
SUCCESS

gates-py3.12
SUCCESS

formal-review-package
SKIPPED (expected)
```

No Critic verdict has been recorded in this checkpoint. **Do not merge PR #14 without the normal short Critic review/verdict.**

## 4. Owner requirement that created PR #14

Owner explicitly required a new package named conceptually **Environment** responsible for notebook/runtime execution.

Required behavior:

1. Significant notebook operations are wrapped with decorators such as:
   `@environment.step(...)`.
2. START/PASS/FAIL is centralized.
3. If a managed notebook step fails:
   - write durable failure information to a persistent log;
   - include error type/message/traceback;
   - flush/fsync log before shutdown;
   - then shut down the Colab runtime as well.
4. Successful completion also shuts down through the same lifecycle owner.
5. Old path module is moved into Environment and renamed around `Routes`.
6. Existing success-only Colab shutdown helper is absorbed into Environment.
7. Implementation is OOP/TDD; use GoF/GRASP sensibly.
8. Working notebook must actually use the path abstraction instead of scattering manual paths.
9. Local/debug execution may explicitly disable shutdown; production defaults are fail and success shutdown.

This direct Owner instruction supersedes the older policy where FAIL left runtime alive.

## 5. PR #14 architecture at current head

New package:

```text
Colab/Infrastructure/Libraries/VoxFlux/environment/
    __init__.py
    console.py
    environment.py
    routes.py
    run_log.py
    runtime.py
```

Responsibilities:

### `Environment`

```text
GoF Facade + GRASP Controller
```

Owns:
- managed-step decorator;
- lifecycle orchestration;
- route facade;
- persistent run log;
- success/failure terminal policy.

### `Environment.step(...)`

Decorator behavior:

```text
START
  -> call managed operation
  -> PASS: persist PASS, continue
  -> FAIL:
       persist FAIL + traceback
       fsync
       print FAIL
       request shutdown(reason="failure")
       re-raise original exception
```

### `RunLog`

Persistent JSONL logger.

Current log path:

```text
Output/Logs/<timestamp>-<run-id>.jsonl
```

Each record is written, flushed and `os.fsync(...)`'d.

### `RuntimeSession`

GoF Strategy contract.

Current implementations:

```text
ColabRuntimeSession
LocalRuntimeSession
```

Colab shutdown attempts:
1. `drive.flush_and_unmount()`;
2. `runtime.unassign()`.

### `Routes` / `RouteManager`

Deployment path ownership moved from:

```text
VoxFlux.core.paths
```

to:

```text
VoxFlux.environment.routes
```

`Manager` renamed to `RouteManager`.

Current Routes include:

```text
RADIX
INPUT
OUTPUT
INFRASTRUCTURE
LOGS
MODELS
LIBRARIES
WHISPER
PARAKEET
AUXILIARY
```

`LOGS = Output/Logs`.

Legacy:
- `VoxFlux.core.paths` removed;
- old `VoxFlux.adapters.colab` success-only shutdown removed;
- old adapters package removed from active code;
- operator console lives under Environment.

## 6. Working notebook state in PR #14

`Colab/VoxFlux.ipynb` is refactored.

After unavoidable bootstrap:
- `Environment` is the single runtime controller;
- `Routes` resolves working paths;
- dependencies, pipeline initialization, input discovery, per-file processing and result verification use `@environment.step(...)`;
- final success calls `environment.complete()`;
- managed failure is supposed to write the persistent FAIL record and disconnect runtime.

One bootstrap literal remains intentionally necessary before VoxFlux can be imported:

```text
/content/drive/MyDrive/VoxFluxSTT/Colab
```

and a single bootstrap join is used to make:
```text
Infrastructure/Libraries
```
importable. After that, runtime paths must come through Environment/Routes.

## 7. TDD / tests in PR #14

New principal test:

```text
Infrastructure/Runtime/Tests/test_environment.py
```

Contract coverage includes:
- RouteManager path resolution;
- PASS logs START/PASS and does not shut down early;
- FAIL persists START/FAIL/SHUTDOWN, traceback and error metadata;
- FAIL invokes runtime shutdown and re-raises the original exception;
- complete() records success and shuts down;
- debug policies can disable shutdown.

Existing path/notebook/operator tests were migrated to Environment API.

Latest CI is green on Python 3.10 and 3.12.

## 8. P0.2 product state — closed

P0.2 is treated as closed as:

```text
Whisper large-v3 + external Silero VAD
```

Production result improved materially:
- no hallucinated subtitle credits before speech;
- real speech begins around 00:53;
- prior 00:53–01:21 loss fixed;
- prior ~03:55–04:25 speech loss fixed;
- synchronization is good;
- "Продолжение следует" / subtitle-credit garbage absent.

Residual item at ~03:09:
- one short reply `"чат не видно"` by a second (female) speaker is still lost/collapsed;
- speaker labels are not implemented.

These are explicitly **not P0.2 blockers anymore**. They moved to P0.3.

## 9. P0.3 — speaker diarization / speaker-aware ASR

Goal:

```text
VAD
  -> speaker diarization
  -> speaker turns
  -> ASR per appropriate turn/group
  -> SRT with speaker labels
```

Do not infer gender automatically. Labels should be neutral initially, e.g.:
`Спикер 1`, `Спикер 2`; later product mapping can name roles.

Acceptance focus:
- second speaker near ~03:09 is represented as a distinct turn;
- lost short `"чат не видно"` reply is tested explicitly;
- compare regular vs exclusive diarization;
- if only regular mode sees the second speaker, overlap handling must be designed explicitly;
- many tiny Whisper calls are risky; group adjacent same-speaker turns and use edge padding;
- compare processing speed with current production baseline (~104 s large-v3+VAD on the reference file, historical diagnostic figure).

### Model / token

Current probe model:

```text
pyannote/speaker-diarization-community-1
```

Owner has been instructed to:
- accept gated-model terms on the exact Community-1 model page;
- expose Colab secret `HF_TOKEN` to the notebook.

## 10. P0.3 probe history

Drive review folder:

```text
VoxFluxSTT/
Infrastructure/Environments/AI/Review/
P0.3/Speaker-Diarization-Probe/
```

Historical v001/v002/v003 are marked `HISTORICAL-DO-NOT-RUN`.

### v003

v003 evidence:

```text
P0.3-speaker-diarization-full-file-probe-v003.json
Drive ID:
1DWP7Eq18PuI7SQug0q8zzecqq2DZ3kyU
```

First v003 blocker was Hugging Face gating (403), later resolved by Owner accepting access.

After access, v003 reached audio processing but failed before diarization result:

```text
ValueError:
requested chunk [00:00:00.000 --> 00:00:10.000]
from AN-V01-part-001 file resulted in 158895 samples
instead of expected 160000 samples.
```

Interpretation:
- not a negative diarization result;
- pyannote file-path/codec chunk seeking over MP3 returned a short chunk;
- regular/exclusive speaker fields remained null;
- inference did not complete.

Useful compatibility evidence from v003:
- pyannote.audio 4.0.7 imports;
- torch and torchaudio remained `2.11.0+cu128`;
- CUDA available;
- pyannote did not replace torch/torchaudio;
- NumPy version changed during dependency installs but fresh subprocess imports were healthy;
- runtime actually used Tesla T4 in that run.

### Current canonical probe: v004

Canonical Drive notebook:

```text
P0.3-SPEAKER-DIARIZATION-PROBE-RUN-ME.ipynb

Drive ID:
1Esx45wQ__fwauHB-ioAIwS4Iqhn5YWhG
```

v004 design:
- uses the **full source file**, not a 20-second diarization input;
- decodes the entire MP3 once to 16 kHz mono PCM/WAV;
- feeds pyannote an in-memory waveform + sample_rate to avoid MP3 random-seek chunk mismatch;
- preserves original full-file time axis;
- then inspects only diagnostic window 03:00–03:20;
- separately reports regular and exclusive speaker detection around 03:09;
- records source/PCM duration and delta;
- checks dependency/Whisper compatibility in a fresh subprocess.

**At this checkpoint v004 has NOT yet produced a successful PROBE SUMMARY.**
Do not claim pyannote detects or misses the woman until v004 actually runs to inference completion.

## 11. Permanent Git -> Drive sync state

PR #13 was accepted and merged earlier.

Permanent synchronization was successfully owner-run against:

```text
commit
97e2038c99c759df4ac135bd66ce6a463844a1be

tree
0a3519f791dc437f9790cb118c21f4f4bd6f0743

CI run
36145677518
```

Successful first full sync summary included:

```text
tracked_file_count: 135
created: 23
updated: 29
unchanged: 83
stale_trashed: 1
class_b_preserved: 438
generated_bytecode_trashed: 42
```

Drive IDENTITY file ID at that sync:

```text
1IP4qY0wsWLcw3CcjiOe6rDPBj6CG6rA8
```

Canonical sync launcher Drive ID:

```text
1RpGlCxLG2xb9dHxMI2u2cJxP0bqZRJ2g
```

Do not run the old experimental notebooks 11/12 from Runtime; they were moved to Warehouse/Archive/Superseded.

## 12. Important next action after recovery

Primary active task is PR #14 Environment infrastructure.

Next sequence:

```text
1. Re-read live PR #14 head and CI (do not assume this checkpoint is newer than live).
2. If PR #14 head is still a3944a0b... and CI is still green:
   - prepare the normal small Critic review transport/diff on Drive;
   - obtain one Critic verdict;
   - fix only concrete blockers if any.
3. Do NOT merge before Critic READY/PASS.
4. After PR #14 is accepted/merged and synchronized to Drive, return immediately to product-visible P0.3.
5. Run/inspect v004 and continue speaker-diarization decision from its actual PROBE SUMMARY.
```

Do not start another unrelated infrastructure project before returning to P0.3.

## 13. Recovery precedence

This checkpoint is controlling over older VoxFlux Creator checkpoints where they conflict.

Always re-check live GitHub/Drive state first. If live state is newer:
- never roll back automatically;
- read newer commits/evidence;
- advance from live state.

