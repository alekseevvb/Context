# VoxFluxSTT Creator checkpoint

role: Creator
prompt_version: 1.8
timestamp_utc: 20261002-091000Z

main_repository: alekseevvb/VoxFluxSTT
main_branch: Genesis
main_head: 49ee5236d35a0d16c885dfe27dc3d431ad0d200e
main_tree: 256a441f2b73253168f6313abc4246c6046a5fb6
main_ci_status: PASS

drive_sync_status: PASS
drive_head: 49ee5236d35a0d16c885dfe27dc3d431ad0d200e
drive_tree: 256a441f2b73253168f6313abc4246c6046a5fb6

## Drive verification
Owner ran Colab/GitHub_Синхронизация.ipynb successfully.
Drive preflight reported 27 tracked allowlisted protected paths and 501 local persistent paths.
Drive verified exact HEAD/tree equal current Genesis.

Independent Drive tree verification:
- Colab/Input/AN-V01-part-001.mp3 present, 4,793,040 bytes.
- Whisper cache preserved: large-v3.pt 3,087,371,615 bytes; medium.pt; small.pt.
- Pyannote cache preserved: models--pyannote--speaker-diarization-community-1.
- UAT P0_4 code refreshed on Drive, including f02_diagnostic_worker.py / f02_diagnostic.py / f02_colab_entrypoint.py.
- Runtime UAT/Data remains present and protected.

## Owner-requested official control
Goal: test whether the official-notebook execution style changes/localizes P0.4-F-02 without a full pipeline rerun.
Official basis: Colab/VoxFlux.ipynb.
Existing primary production diagnostic remains the authorized F02 narrow diagnostic.

Control design:
- control clip: 174-204 s
- controlling denominator: 184-194 s
- Lane A: direct Whisper with minimal upstream-like kwargs
- Lane B: direct Whisper with current VoxFlux production ASR kwargs
- Lane C: current VoxFlux parser applied to the exact Lane-B raw result
- no external VAD
- no Pyannote
- no speaker grouping
- no full pipeline.run()

Interpretation:
- A correct / B wrong -> ASR policy or kwargs
- A and B raw correct / C wrong -> provider parser
- A/B/C correct / production diagnostic wrong -> VAD, diarization, grouping, or clip construction
- A wrong -> upstream Whisper/model/audio behavior

## PR #41
branch: diagnostic/p04-f02-official-control
base: Genesis @ 49ee5236d35a0d16c885dfe27dc3d431ad0d200e
pr: 41
state: OPEN / DRAFT
current_head: be516dc39aa28f1f78c0d1d9c935a27a2cda3abd
current_tree: 0d297502b3a843c18521f65c7863eceace68e0d6
behind_by: 0
mergeable: true

Changed files:
1. Infrastructure/Runtime/UAT/P0_4/f02_official_control_worker.py
2. Infrastructure/Runtime/Tests/test_p04_f02_official_control.py
3. Infrastructure/Runtime/Colab/2026.10.02/06. P0.4 F02 Official Control. Colab.ipynb
4. Infrastructure/Runtime/Tests/fixtures/path_scan_allowlist.json

Initial CI:
head 98ecd77557ae26d770149c1228b2971e6e5a8698
run 36987344931
Py3.12 exposed two diagnostic-only defects:
- wrong test expectation for word 183.5-184.1, which correctly overlaps 184-194
- missing PATH-15 authorization for typed runtime path fields in the new UAT worker

Corrections:
- fixed test expectation, not overlap implementation
- removed unused CLI Path(sys.argv...) surface from import-only worker
- added one narrow PATH-15 UAT worker rule; no deployment literal allowlisted

Exact-head GREEN:
head be516dc39aa28f1f78c0d1d9c935a27a2cda3abd
run 36987678986
Python 3.10: canonical install PASS; 194 passed; Ruff PASS; mypy PASS 56 files; SHA256 PASS
Python 3.12: canonical install PASS; 194 passed; Ruff PASS; mypy PASS 56 files; SHA256 PASS

## current task
Await narrow Critic review of PR #41 exact SHA be516dc39aa28f1f78c0d1d9c935a27a2cda3abd.

## next action if READY
1. merge exact PR #41 with unchanged-head guard
2. verify post-merge Genesis CI
3. run GitHub_Синхронизация.ipynb again so Drive receives the new diagnostic notebook/worker
4. verify Drive exact HEAD/tree
5. run Infrastructure/Runtime/Colab/2026.10.02/06. P0.4 F02 Official Control. Colab.ipynb on GPU
6. read P0.4-F02-official-control-v001.json and compare lanes A/B/C with existing production F02 diagnostic

Do not merge PR #41 before Critic READY.
Do not modify production pipeline for this experiment.
