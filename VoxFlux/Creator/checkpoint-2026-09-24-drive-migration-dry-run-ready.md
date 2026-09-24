# VoxFlux Creator checkpoint — Drive migration DRY RUN ready

**Date:** 2026-09-24
**Role:** Creator
**Status:** controlling checkpoint

## Accepted code baseline

```text
VoxFluxSTT/Genesis:
698d033b0bac1161ed393958ee1e97ca9f70f829
```

No current formal P0.1 code candidate exists.

Historical P0.1 PRs #4/#5/#6 are closed/superseded.

## Drive layout design verdict

```text
CRITIC-VERDICT-DRIVE-TARGET-LAYOUT-52db2ef7.md

Drive ID:
1IS1aPPruLs09XKGOWaq8J8Xhf9AHc2Ge

verdict:
READY_WITH_CONDITIONS
```

Critic conditions C1-C5 are integrated in:

```text
branch:
drive-layout-design

commit:
3c90ccbe4ffe68613bb8af461d25e0b422d31a8f

file:
Infrastructure/Documentation/md/DRIVE-TARGET-LAYOUT.md
```

No extra design-verdict round is required.

## Transfer zone

```text
Applications/VoxFluxSTT/
Infrastructure/Environments/AI/Transfer/

Drive folder ID:
1L2bgzvZgeYjNmv8P9pUipigOjpSOaLLV
```

Existing publication notebook was moved there using Drive API, preserving ID:

```text
PUBLISH-DRIVE-LAYOUT-001.ipynb

Drive ID:
1_K6qXiXWgCtkVwVRVB5S-YiDzcFe94J6
```

## Canonical DRY RUN

```text
Git:
Infrastructure/Runtime/Transfer/DRIVE-MIGRATION-DRY-RUN-001.ipynb

commit:
e5b9ad117da70069a9062bfcefd454c5d2657477

blob:
720fb60612f9ac6d145262525ab6e4273385bcb0
```

The canonical notebook has no APPLY operations.

It inventories Drive, hashes Whisper weights, hashes the protected SRT,
compares both duplicate backups, checks target conflicts, and publishes a
split ID-preserving migration plan under Review/Migration.

## Drive launcher

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

The launcher fetches and verifies the exact canonical Git commit/blob above,
then executes its sole code cell.

## Current authorization

Allowed:

```text
RUN DRY RUN ONLY
```

Forbidden:

```text
APPLY
runtime Drive relocation
Whisper/Parakeet relocation
Input/Output/UAT relocation
VoxFluxSTT-Evidence relocation
backup relocation/deletion
old roadmap relocation
P0.1 code sync
Models/pip deletion
legacy Applications/VoxFlux archival
```

## Next action

User runs the Drive launcher and sends the complete output.

Then Creator:
1. reads the split DRY RUN evidence from Drive;
2. verifies MANIFEST/SHA256SUMS;
3. prepares Critic handoff;
4. waits for Critic review of the DRY RUN plan before any APPLY work.
