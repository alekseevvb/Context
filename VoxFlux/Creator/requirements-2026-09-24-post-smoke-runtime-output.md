# VoxFlux post-smoke runtime/output requirements

**Date:** 2026-09-24
**Status:** DEFERRED_UNTIL_P0_0_SMOKE_CLOSE
**Source:** user requirements captured during Run 1

Do not modify accepted P0.0 code before Run 2 and durable Critic smoke verdict.

## R1 — timing telemetry

For every processed media object, report at minimum:

```text
source media duration
processing wall-clock time
real-time factor (RTF = processing_time / media_duration)
throughput factor (x realtime = media_duration / processing_time)
model
device / accelerator
```

Preferred console example:

```text
File: AN-V01-part-001.mp3
Duration: 00:42:18
Processing: 00:03:57
Speed: 10.7x realtime
RTF: 0.093
Model: Whisper medium
Device: NVIDIA L4
```

Use a monotonic timer such as `time.perf_counter()` for processing time.
Media duration should be obtained from the actual media container/audio stream,
preferably through a single media-probe abstraction rather than inferred from the SRT.

## R2 — directory batch processing

The current notebook already enumerates supported files in `Input/` and processes
all discovered items in a loop. Preserve this behavior and make it explicit in the
runtime summary:

```text
files discovered
files processed successfully
files failed
total media duration
total processing time
aggregate realtime factor
```

A failure on one object should later be considered for isolation so one bad item
does not necessarily destroy the entire batch; exact failure policy is a separate decision.

## R3 — run-scoped output directory

Each notebook execution that processes one or more source objects should create one
run directory under `Output/`.

User-requested naming direction:

```text
YYYY.MM.DD HH-mm. Model - <model name with parameters>
```

Working normalized example:

```text
2026.09.24 15-30. Model - Whisper medium
```

The exact parameter serialization is not yet frozen. Before implementation decide:
- which parameters belong in the folder name;
- maximum/path-safe length;
- stable ordering;
- characters forbidden by filesystems/Drive;
- whether detailed parameters belong in a manifest rather than all in the name.

Preferred architecture direction:
- human-readable short directory name;
- complete reproducibility metadata in a machine-readable run manifest inside the directory.

Proposed output shape:

```text
Output/
└── 2026.09.24 15-30. Model - Whisper medium/
    ├── RUN.json
    ├── AN-V01-part-001.srt
    ├── other-file.srt
    └── ...
```

## R4 — run manifest

Recommended `RUN.json` fields:

```text
run_id
started_at
finished_at
model
model_parameters
accelerator
device_name
input_count
success_count
failure_count
total_media_duration_seconds
total_processing_seconds
aggregate_rtf
files[]
```

Each file record should include:
- source filename;
- source media duration;
- processing time;
- RTF / x-realtime;
- output path;
- success/failure;
- error code/message if failed.

## Process re-entry

After:

```text
P0.0 Run 2 PASS
AND
durable CRITIC-VERDICT-P0.0-smoke.md
```

return first to:
1. `decision-2026-09-24-post-smoke-path-module-refactor.md`;
2. this runtime/output requirements file;
3. then define the exact P0.1 implementation scope.


## R5 — automatic Colab runtime release after successful run

After the notebook has completed all intended processing and all durable outputs
have been flushed to Drive, the Colab runtime should release itself automatically.

Reference implementation found in Regenesis:

```text
Regenesis/XX. Miscellaneous/Experiments/REG-EXP-CPU2-001/
Regenesis-v0.143-cpu2-capacity-experiment-prereg.6.zip
  -> reference/Regenesis-v0.143-colab-package.1.zip
  -> runtime-lib/regenesis_runtime/adapters/colab.py
```

Reference behavior:

```python
from google.colab import runtime
runtime.unassign()
```

The reference adapter emits a terminal shutdown message, waits for a short grace
period, then calls `runtime.unassign()`.

VoxFlux requirement:
- do NOT disconnect before all SRT files are written;
- do NOT disconnect before the run manifest / telemetry is written and flushed;
- emit a final `STOP` / runtime-release status line before disconnect;
- allow a short grace period for console and Drive writes to settle;
- automatic disconnect occurs on terminal successful completion;
- on a processing/finalization failure, keep the runtime alive for diagnostics
  unless a later explicit policy says otherwise;
- the shutdown action must be isolated behind a runtime adapter/helper instead of
  scattering direct Colab-specific calls throughout the pipeline.

## R6 — Linux-boot-style structured console output

The console presentation should be adapted from the verified Regenesis
`RedHatConsoleFormatter` rather than invented from scratch.

Reference source:

```text
Regenesis/XX. Miscellaneous/Experiments/REG-EXP-CPU2-001/
Regenesis-v0.143-cpu2-capacity-experiment-prereg.6.zip
  -> reference/Regenesis-v0.143-colab-package.1.zip
  -> runtime-lib/regenesis_runtime/formatter.py
```

Reference formatter SHA-256:

```text
c12d31fbee23635e239d0f8d6f36211d7e8ab0d633a616881b7de2353cddf893
```

Reference layout:

```text
HH:MM:SS  [  STATUS  ]  TASK/LABEL                                      DURATION
```

Reference semantic statuses:

```text
START       -> ....
PASS        -> OK
FAIL        -> FAILED
TIMEOUT     -> TIME OUT
INFRA_ERROR -> INFRA
WAIT        -> WAIT
INFO        -> INFO
STOP        -> STOP
```

Reference color semantics:
- PASS: green;
- FAIL/TIMEOUT/INFRA: red;
- WAIT: yellow;
- INFO/STOP: cyan;
- START: dim.

VoxFlux adaptation should keep the same visual grammar while using VoxFlux task
names and runtime semantics.

Working target example:

```text
15:42:10  [   ....   ]  Mount Google Drive
15:42:11  [    OK    ]  Google Drive mounted                                  1.02s
15:42:11  [   INFO   ]  Project root: /content/drive/MyDrive/Applications/VoxFlux
15:42:11  [   INFO   ]  Model cache: .../Infrastructure/Models
15:42:12  [   ....   ]  Install dependencies
15:42:19  [    OK    ]  Dependencies ready                                   7.14s
15:42:19  [   ....   ]  Load Whisper medium / CUDA
15:42:27  [    OK    ]  Whisper medium ready                                 8.21s
15:42:27  [   INFO   ]  Files discovered: 4
15:42:27  [   ....   ]  [1/4] AN-V01-part-001.mp3
15:46:12  [    OK    ]  [1/4] AN-V01-part-001.mp3                          225.14s
15:46:12  [   INFO   ]    media=00:42:18 processing=00:03:45 speed=11.28x RTF=0.089
15:46:12  [   ....   ]  [2/4] example.flac
...
15:57:03  [    OK    ]  Batch complete
15:57:03  [   INFO   ]  files=4 success=4 failed=0 media=02:11:08 processing=00:14:35
15:57:03  [   INFO   ]  output=Output/2026.09.24 15-42. Model - Whisper medium/
15:57:03  [   STOP   ]  Colab runtime release requested
```

Requirements:
- fixed columns so status lines do not jump visually;
- ANSI colors when the console supports them;
- deterministic plain-text fallback without ANSI for evidence/log files;
- timestamps on every status line;
- durations on completed timed operations;
- nested/indented detail lines for per-file timing and metadata;
- final batch summary before runtime shutdown;
- console semantics and machine-readable `RUN.json` must describe the same run;
- formatting is presentation only and must not own pipeline/business logic.

The implementation should respect the project convention already recorded:
one primary class per file and, where practical, one-word file/class names.
A dedicated console/output package should be designed before coding rather than
placing this formatter into unrelated ASR classes.

## R7 — shutdown/evidence ordering invariant

The terminal ordering for a successful Colab run must be:

```text
1. finish processing all discovered inputs;
2. write every SRT/result;
3. compute timing/batch summary;
4. write and close RUN.json;
5. ensure Drive-visible output is complete;
6. emit final PASS summary;
7. emit STOP runtime-release line;
8. short grace delay;
9. google.colab.runtime.unassign().
```

A disconnect before steps 1-7 is a run failure, not a successful shutdown.


## R8 — executable-cell / stage lifecycle logging

Every executable notebook cell that represents a logical runtime stage must emit
a structured lifecycle record instead of relying only on ad-hoc `print()` output.

The durable identity should be a stable stage ID/name, not a fragile physical cell
number, because cells can be reordered without changing the underlying operation.

Required lifecycle:

```text
START -> PASS
      -> FAIL
```

Each stage record should include, where applicable:

```text
timestamp
stable stage id
human-readable stage name
status
elapsed duration
selected runtime metadata
error summary / exception type on failure
```

Recommended notebook stages:

```text
BOOT      Mount / bind Google Drive
PATHS     Resolve deployment paths and sys.path
DEPS      Install / validate dependencies
DEVICE    Detect accelerator / CUDA device
MODEL     Initialize ASR model
DISCOVER  Discover supported Input files
RUN       Process batch
FINALIZE  Write SRT + RUN.json + summaries
SHUTDOWN  Release Colab runtime
```

Working Linux-boot-style presentation:

```text
16:20:01  [   ....   ]  BOOT      Mount Google Drive
16:20:02  [    OK    ]  BOOT      Google Drive mounted                     1.14s
16:20:02  [   ....   ]  PATHS     Resolve deployment paths
16:20:02  [    OK    ]  PATHS     Deployment paths ready                   0.03s
16:20:02  [   ....   ]  DEPS      Install dependencies
16:20:09  [    OK    ]  DEPS      Dependencies ready                       7.01s
16:20:09  [   ....   ]  DEVICE    Detect accelerator
16:20:09  [    OK    ]  DEVICE    NVIDIA L4 / CUDA                         0.09s
16:20:09  [   ....   ]  MODEL     Initialize Whisper medium
16:20:17  [    OK    ]  MODEL     Whisper medium ready                     8.14s
16:20:17  [   INFO   ]  DISCOVER  Files discovered: 4
...
16:35:43  [    OK    ]  FINALIZE  Results flushed to Drive                0.31s
16:35:43  [   STOP   ]  SHUTDOWN  Colab runtime release requested
```

Rules:
- markdown-only notebook cells do not require runtime lifecycle events;
- every executable orchestration cell/stage must have at least one terminal status;
- a stage that raises must emit `FAIL` before propagating the exception when safe;
- internal library operations may emit nested detail lines through the same logger,
  but should not create an independent competing formatting system;
- cell/stage logs, per-file logs, batch summary, and `RUN.json` must refer to the
  same run identity;
- formatting must be centralized in the planned console module and must not be
  reimplemented separately in notebook cells;
- plain-text evidence mode must preserve the same semantic events without ANSI codes.

This requirement should be implemented together with R6, not as a second logger.
