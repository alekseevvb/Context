# VoxFluxSTT Creator checkpoint

role: Creator
prompt_version: 1.8
timestamp_utc: 20261002-094700Z

main_repository: alekseevvb/VoxFluxSTT
main_branch: Genesis
main_head: 418d70b9568303c2278f5fdefc803bde921a780c
main_tree: 0d297502b3a843c18521f65c7863eceace68e0d6
main_ci_status: PASS

drive_sync_status: PASS
drive_head: 418d70b9568303c2278f5fdefc803bde921a780c
drive_tree: 0d297502b3a843c18521f65c7863eceace68e0d6

Owner ran second GitHub_Синхронизация.ipynb successfully.
Drive preflight: 27 tracked allowlisted protected paths, 502 local persistent paths.
Drive sync verified exact HEAD/tree.

## reviewed Official Control identity on Drive
Notebook path:
Infrastructure/Runtime/Colab/2026.10.02/06. P0.4 F02 Official Control. Colab.ipynb
Drive file id: 1pDYSMEbiajgk8Lge3i4SV61LsM6DvyYC
Git blob SHA on Drive bytes: c0848a771d0a25389aa62755c3d69eb120d432a8
GitHub Genesis blob SHA: c0848a771d0a25389aa62755c3d69eb120d432a8
BYTE IDENTITY: PASS

Worker path:
Infrastructure/Runtime/UAT/P0_4/f02_official_control_worker.py
Drive file id: 1-w-d5ThEvnAvNyh4RJIfuzvQxN2DtHoX
Git blob SHA on Drive bytes: 026a16a40682204e679ea3187e07d0d2572ef2b8
GitHub Genesis blob SHA: 026a16a40682204e679ea3187e07d0d2572ef2b8
BYTE IDENTITY: PASS

## Official Control ready state
Run on GPU:
MyDrive/VoxFluxSTT/Infrastructure/Runtime/Colab/2026.10.02/06. P0.4 F02 Official Control. Colab.ipynb

Expected evidence output:
Infrastructure/Runtime/UAT/Data/P0_4/F02_Official_Control/P0.4-F02-official-control-v001.json

Control design:
- Lane A upstream-like direct Whisper
- Lane B direct Whisper with current VoxFlux ASR kwargs
- Lane C current VoxFlux parser over exact Lane-B raw result
- control window 174-204 s
- denominator 184-194 s
- no VAD/Pyannote/grouping/full pipeline

Interpretation caution:
If A/B differ, inspect raw result for fallback temperatures > 0 before attributing difference solely to policy.

## next action
Owner runs Official Control notebook on GPU and returns output or simply reports completion.
Creator then reads evidence JSON from Drive and localizes the first mismatch.

context_head_before_save: b055170d547b22c452aa9b23693d38dc8f3eb96c
