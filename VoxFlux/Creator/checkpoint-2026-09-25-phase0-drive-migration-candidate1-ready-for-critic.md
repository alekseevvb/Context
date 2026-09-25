# VoxFlux Creator checkpoint — Phase-0 Drive migration Candidate.1 ready for Critic

## Controlling state

Role: Creator.

Repository:
`alekseevvb/VoxFluxSTT`

Branch:
`phase-0-drive-migration-candidate-1`

Base / parent:
`3a2c38d0d659901cd726f6f7da6a289539e4b6ae`

Candidate:
`82e4b32f53b16a5b86753662bf849c2d982b401c`

Tree:
`9dbfcd4d25e0dfce82e7c5672f284208753263eb`

PR:
`#7` draft; do not merge before Critic acceptance.

## Structural verdict

`DRIVE-TARGET-LAYOUT v2 — READY_WITH_CONDITIONS`

Structure is frozen.

B1 implemented:
- Colab root = `/content/drive/MyDrive/VoxFluxSTT/Colab`;
- P0.1 explicit Whisper `download_root`;
- no VoxFlux `XDG_CACHE_HOME` override.

B2 implemented:
- stale Class A = tracked(previous IDENTITY commit) - tracked(new commit);
- no IDENTITY on first sync => empty stale set;
- invalid/unresolvable IDENTITY => FAIL.

## Final candidate CI

GitHub Actions run:
`36076054473`

Python 3.10:
pytest PASS / Ruff PASS / Mypy PASS / repository SHA256 PASS.

Python 3.12:
pytest PASS / Ruff PASS / Mypy PASS / repository SHA256 PASS.

formal-review-package:
PASS.

## Review package

Canonical ZIP:
`phase-0-drive-migration-candidate-1-review-package.zip`

SHA-256:
`97e133e46531a986adb4731bf4d58b41df88332dd5a67b9bbbdef18a88239dca`

Drive review folder:
`Applications/VoxFluxSTT/Infrastructure/Environments/AI/Review/P0-Migration/Candidate-001/82e4b32f53b16a5b86753662bf849c2d982b401c/`

Folder ID:
`14YhDKnVtGIB0pKxm8ZwMQ6P0iV4Rnhar`

ZIP ID:
`1efGby5iDG3TqVZi-m0Fe1LmbHAHaZr4g`

sidecar ID:
`1v0FaewPRYa2Wfc-lEHDEFqpZPxG0LgCR`

MANIFEST ID:
`187lC7KlJyWGunncCzWzoJV0Gi6VFWLe-`

CANDIDATE ID:
`18W3vzZosKKHHiErpiyJIu6km2v2kt7v7`

Drive ZIP readback SHA-256 matches canonical ZIP.

## Creator -> Critic

Controlling timestamp-correction message:
`2026-09-25T00-11-03Z__phase-0-drive-migration-candidate-1-handoff-timestamp-correction.md`

Drive ID:
`1amqTfArqdavKWbMrZisLbRpfS5gBzCsu`

It references the earlier immutable handoff whose filename had an incorrect future UTC timestamp.

## Important historical identity

`Review/P0-Migration/Candidate-001/b57f87476d799e1539661ceeefe3197eec547ce2/`

is superseded pre-final evidence and is NOT controlling.

## Next action

Critic performs independent review of Candidate.1.

Until Critic accepts:
- DRY-RUN-002 = BLOCKED
- APPLY = BLOCKED
- PR merge = BLOCKED
