# VoxFlux Creator checkpoint — P0-47 PASS, P0.0 smoke pending

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

Candidate.2 Critic verdict remains:

```text
READY
CRITIC-VERDICT-P0.0-Candidate.2.md
Drive ID 1tiLW6otGyD8UDVk6v9hAMobkSDa06ELO
```

## P0-47

```text
status:
PASS

synchronized surfaces:
17 / 17 exact SHA-256 PASS
```

Evidence:

```text
Applications/VoxFluxSTT-Evidence/P0.0/Candidate-02/
698d033b0bac1161ed393958ee1e97ca9f70f829/
p0-47-drive-sync/

Drive folder ID:
19RI5VTsUGSmzSbAXAoPRyfSkS3CLyvrA

SHA256SUMS:
ed2f386430e5120940ccfecc81e89ad7ca9f5f136d1c3077abdd89e481864369

Drive readback:
DRIVE_SHA256SUMS_BYTE_IDENTICAL
```

Source came from verified GitHub Actions formal artifact ID `10788710988`,
containing exact Candidate.2 source ZIP for commit `698d033b...`.

## Backup

Created before code replacement:

```text
Applications/VoxFlux-backup-pre-P0-47-2026-09-24/
Drive ID:
1VxZYYmGgE4olqi44_Y2AlR1CJj_mgTQQ
```

Backup census:

```text
40 files
12 folders
```

The old notebook and complete old `Infrastructure/Libraries/VoxFlux/`,
including runtime `__pycache__`, are preserved.

Google Drive connector cannot delete folders and does not support overwrite
uploads. Therefore old `__pycache__` and the old whole library were moved
outside deployment to backup/quarantine before fresh exact-source upload.

## Renames

```text
Infrastructure/Libraries/Auxilary -> Auxiliary
Infrastructure/Models/Parackeet -> Parakeet
```

Both source folders contained only `.gitkeep` before the operation.
The folder IDs were preserved by in-place rename; exact Candidate.2
`.gitkeep` bytes were then uploaded.

## Protected runtime

Not synchronized/replaced:

```text
Input/
Output/
UAT/
Models/whisper/
Models/pip/
Models/Whisper/
```

Pre-smoke `Models/whisper/`:

```text
large-v3.pt
small.pt
```

`medium.pt` is absent before smoke.

## Corrected smoke contract

First fresh Colab run:
- all cells pass without manual code edits;
- expected deployment/cache paths are printed;
- `medium.pt` downloads and appears specifically in `Infrastructure/Models/whisper/`;
- an SRT is created;
- no unexpected directories are created.

Second run after runtime restart:
- all cells pass again;
- `medium.pt` is reused from `Models/whisper/`;
- model is not downloaded again;
- SRT processing succeeds;
- no unexpected directories are created.

The canonical `Models/Parakeet/` already exists after the intentional P0-47
rename; smoke must not create a duplicate or other unintended Parakeet path.

Old-vs-new SRT byte comparison is not a smoke requirement.

Protocol:

```text
Applications/VoxFluxSTT-Evidence/P0.0/Candidate-02/
698d033b0bac1161ed393958ee1e97ca9f70f829/
smoke/SMOKE-PROTOCOL.md

Drive file ID:
11hEVlTVrE_4sF3scbF-FFCaYPcP4sfW9
```

## Frontier

```text
NEXT-006:
CLOSED

NEXT-007:
P0-47 PASS
P0.0 smoke NOT RUN
OPEN

P0.1:
BLOCKED
```

P0.1 opens only after:
1. smoke PASS;
2. durable Critic verdict file `CRITIC-VERDICT-P0.0-smoke.md`.

Full Phase-0 UAT remains NOT RUN and waits for P0.1-P0.3.
