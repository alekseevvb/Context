# VoxFlux Creator checkpoint — Drive layout review first

**Date:** 2026-09-24
**Role:** Creator
**Status:** controlling checkpoint

## Accepted baseline

```text
Genesis:
698d033b0bac1161ed393958ee1e97ca9f70f829
```

P0.0 remains CLOSED by durable Critic PASS.

## P0.1 candidate reset

```text
PR #4:
CLOSED / SUPERSEDED

PR #5:
CLOSED / SUPERSEDED WIP

PR #6:
CLOSED / SUPERSEDED WIP

current formal P0.1 candidate:
NONE
```

Retained architectural requirements:
- route names are `WHISPER` and `PARAKEET`;
- final Routes model uses parent names instead of numeric indexes;
- final consolidated change-set carries no compatibility aliases;
- P0.1 uses explicit Whisper `download_root`;
- Drive restructure and model-weight migration are one operation.

## Current design branch

```text
branch:
drive-layout-design

target-layout + roadmap source commit:
52db2ef7c1b2c1cb171006083dac16b50f3bbc5d

transfer notebook commit:
8235c55ea7d6fda93b1b4a9cea4912737e68011e
```

Files:

```text
Infrastructure/Documentation/md/DRIVE-TARGET-LAYOUT.md
Infrastructure/Documentation/md/ROADMAP.md
Infrastructure/Runtime/Transfer/PUBLISH-DRIVE-LAYOUT-001.ipynb
```

Transfer notebook rules:
- exact Git commit pin;
- private GitHub token from Colab secret `GITHUB_TOKEN`;
- writes only to Review and Roadmap zones;
- immutable/conflict-fail behavior;
- writes MANIFEST.json and SHA256SUMS;
- flushes/unmounts Drive after PASS.

Authorized target roots:

```text
Applications/VoxFluxSTT/Infrastructure/Environments/AI/Review/
Applications/VoxFluxSTT/Infrastructure/Environments/AI/Roadmap/
```

Forbidden:
- Colab/Input
- Colab/Output
- Colab/Infrastructure/Models
- Colab/Infrastructure/Libraries
- UAT
- backups

## Next action

User runs exact notebook commit in Colab.

After run:
1. Creator reads Drive output;
2. verifies manifest/hashes;
3. prepares Critic message;
4. waits for Critic approval of target Drive tree;
5. only then designs the unified DRY RUN/APPLY migration notebook.
