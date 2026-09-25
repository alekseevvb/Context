# VoxFlux Creator checkpoint — P0.1 Candidate.1 awaiting independent Critic review

**Date:** 2026-09-24
**Role:** Creator
**Status:** controlling Creator checkpoint

## Accepted baseline

```text
VoxFluxSTT/Genesis:
698d033b0bac1161ed393958ee1e97ca9f70f829

tree:
c60ff9bea5fcd386a27a85ded41562985dd522a8
```

P0.0 is closed by durable Critic verdict:

```text
CRITIC-VERDICT-P0.0-smoke.md
Drive ID:
1wZ9WrJFpb9vpicyGFflbMCjBJcU-NXcI

verdict:
PASS
```

## P0.1 Candidate.1

```text
branch:
phase-0-p01-cache-path-cleanup

commit:
863b104e752a03c80abd54abfa9e8b0220166f9d

tree:
3335b19ab1352f302c6bcc726089ea12fff88a26

base:
698d033b0bac1161ed393958ee1e97ca9f70f829

draft PR:
https://github.com/alekseevvb/VoxFluxSTT/pull/4
```

Implemented scope:
1. `core/paths/` reduced to two real concepts:
   - `layout.py -> Layout`
   - `manager.py -> Manager`
   - P0.0 names remain compatibility aliases only.
2. P0-F-001 closed:
   - narrowed notebook/manage-context PATH-15 allowlist;
   - mutation test proves `PROJECT_ROOT / 'Input'` is rejected.
3. P0-F-002 closed:
   - dead scanner exclusions removed;
   - regression test requires every remaining exclusion to match a real candidate.
4. P0.1 model cache code:
   - notebook no longer sets `XDG_CACHE_HOME`;
   - canonical configured model dir is `Infrastructure/Models/Whisper`;
   - pipeline passes `config.paths.models_dir` to `WhisperTranscriber`;
   - provider calls `whisper.load_model(..., download_root=models_dir)`;
   - generated context wording no longer describes Models as XDG cache.
5. Fail-closed one-time migration tool:
   - pre-hash SHA-256;
   - `whisper -> whisper-p01-migration -> Whisper`;
   - post-hash verification;
   - rollback on verification failure.

## Verification

```text
P0 Path Contract Gates:
run 36013443813
Python 3.10 PASS
Python 3.12 PASS

P0.1 Model Cache Review:
run 36013443478
verify-py3.12 PASS
p01-review-package PASS

pytest:
61 passed

Ruff:
PASS

mypy:
0 issues / 26 source files

repository SHA256:
PASS
```

## Formal review package

```text
Drive folder:
Applications/VoxFluxSTT-Evidence/P0.1/Candidate-01/
863b104e752a03c80abd54abfa9e8b0220166f9d/review/

folder ID:
1mhBcFqSpiNj3iDvjs3yNbDzXXICa35PZ

review ZIP:
P0.1-model-cache-review-package.zip

Drive file ID:
18ezwOO9wP9QM32jrQQif_UhveE31ix4A

size:
216026 bytes

SHA-256:
a8ec96dd89ccd375de11a9fca8ac9240e44b1da9dac31ef47718a223cb1b04ec
```

GitHub review artifact:

```text
workflow run:
36013443478

artifact ID:
10813412251

outer artifact digest:
c1d1800e3a462f632b7962326027336c3391889e120e5afab620597111766f07

inner review ZIP SHA-256:
a8ec96dd89ccd375de11a9fca8ac9240e44b1da9dac31ef47718a223cb1b04ec
```

The ZIP contains:
- REVIEW.md
- CHANGELOG.md
- identity.txt
- p01.patch
- exact candidate source ZIP
- verification evidence
- internal SHA256SUMS.

## Current Drive runtime state remains untouched by P0.1 migration

```text
Infrastructure/Models/whisper/
large-v3.pt
small.pt
medium.pt

Infrastructure/Models/Whisper/
canonical directory exists but migration of legacy weights has NOT run

Infrastructure/Models/pip/
still exists
contains pip-cache structure including wheels/ and http-v2/
```

The connector cannot download even `small.pt` because of its 256 MiB stream limit.
Exact weight SHA-256 is therefore a post-review, pre-mutation Colab/mounted-Drive gate.

## Fail-closed guard

Until durable independent Critic READY on P0.1 Candidate.1:

```text
MERGE TO GENESIS: FORBIDDEN
DRIVE CODE SYNC: FORBIDDEN
WEIGHT MIGRATION: FORBIDDEN
Models/pip DELETE: FORBIDDEN
```

## Next action

Independent Critic review of:

```text
P0.1-model-cache-review-package.zip
Drive ID:
18ezwOO9wP9QM32jrQQif_UhveE31ix4A
```

If and only if Critic verdict is READY:
1. re-check live Context/Genesis/feature HEADs;
2. fast-forward/merge exact accepted P0.1 candidate as authorized by the verdict;
3. hash-verified sync candidate code to Drive;
4. compute pre-migration SHA-256 on mounted Drive;
5. execute reviewed fail-closed weight migration;
6. verify post-migration SHA-256 identical;
7. preserve evidence;
8. remove `Models/pip/` only after evidence proves it is disposable cache;
9. run P0.1 behavior check showing no model re-download from canonical `Models/Whisper`.
