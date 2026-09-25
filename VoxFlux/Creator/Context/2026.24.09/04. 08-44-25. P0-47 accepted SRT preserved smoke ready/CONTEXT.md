# VoxFlux Creator checkpoint — P0-47 accepted, historical SRT preserved, smoke ready

**Date:** 2026-09-24
**Role:** Creator
**Status:** controlling Creator checkpoint

## Code identity

```text
repo:
alekseevvb/VoxFluxSTT

Genesis:
698d033b0bac1161ed393958ee1e97ca9f70f829

tree:
c60ff9bea5fcd386a27a85ded41562985dd522a8

feature phase-0-path-contract:
698d033b0bac1161ed393958ee1e97ca9f70f829
```

## Candidate.2

```text
Critic candidate verdict:
READY

file:
CRITIC-VERDICT-P0.0-Candidate.2.md

Drive ID:
1tiLW6otGyD8UDVk6v9hAMobkSDa06ELO
```

## P0-47

```text
Creator result:
PASS

Critic verdict:
ACCEPTED

Critic verdict file:
CRITIC-VERDICT-P0-47.md

Drive ID:
1V64e4l-oeJnJZNGJdbaHl-VWqH_nkqVP
```

Evidence folder:

```text
Applications/VoxFluxSTT-Evidence/P0.0/Candidate-02/
698d033b0bac1161ed393958ee1e97ca9f70f829/
p0-47-drive-sync/

Drive ID:
19RI5VTsUGSmzSbAXAoPRyfSkS3CLyvrA
```

## Mandatory smoke Gate 0

Historical output before smoke:

```text
Output/AN-V01-part-001.srt
size:
13937 bytes

SHA-256:
85b357dd5b8d0982357674e42c3e810a4f4b0c8f096e1ebeb3c6e6c887785084
```

Protected before Run 1:

```text
UAT/AN-V01-part-001.srt
Drive ID:
1WihQVcRn7cpH0Xn09dAfPopMwT1CvvLF

size:
13937 bytes

SHA-256:
85b357dd5b8d0982357674e42c3e810a4f4b0c8f096e1ebeb3c6e6c887785084

status:
BYTE_IDENTICAL / PRESERVED
```

Do not overwrite the UAT copy during smoke.

## Pre-smoke model baseline

```text
Infrastructure/Models/whisper/
large-v3.pt
small.pt

medium.pt:
ABSENT
```

Run 1 must download `medium.pt` into this exact directory.
After runtime restart, Run 2 must reuse it with no model-download progress.

## Smoke protocol

Current protocol:

```text
Applications/VoxFluxSTT-Evidence/P0.0/Candidate-02/
698d033b0bac1161ed393958ee1e97ca9f70f829/
smoke/SMOKE-PROTOCOL.md

Drive ID:
1HOkRlEMnfA500v4BZDAR2I3IBIdx3Z1m

SHA-256:
3643bc62c6eb48ff8853c90a7896b7b76d7f791a6ed0f903620f5eeade22e487
```

The previous protocol is retained as historical:

```text
SMOKE-PROTOCOL-pre-critic-SRT-precondition.md
Drive ID:
11hEVlTVrE_4sF3scbF-FFCaYPcP4sfW9
```

## P0.2 preliminary observation

After smoke output is generated, inspect the first five minutes of the new
`Output/AN-V01-part-001.srt` for the phrase:

```text
Продолжение следует
```

This observation is non-blocking for the P0.0 smoke verdict and is preliminary evidence for P0.2.

## Frontier

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

P0.1 opens only after:
1. actual Colab Run 1 passes;
2. actual restarted-runtime Run 2 passes;
3. Critic publishes durable `CRITIC-VERDICT-P0.0-smoke.md`.

Full Phase-0 UAT remains NOT RUN and waits for P0.1-P0.3.
