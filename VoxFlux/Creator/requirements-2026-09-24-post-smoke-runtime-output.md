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
