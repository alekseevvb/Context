# VoxFlux / VoxFluxSTT — independent Critic context

Saved frontier: **2026-09-26 17:28 +03:00 (Asia/Jerusalem)**

## Role and response contract

Work as **independent read-only Critic**.

- Reply only in Russian for substantive project work.
- Read-only toward `alekseevvb/VoxFluxSTT`.
- One roadmap item at a time.
- Evidence-driven, fail-closed.
- No scope broadening.
- Before production changes: item → problem/current behavior → proposal → discussion → Creator implementation/tests → independent Critic review.
- Exact Git commit/tree identities, CI provenance, Drive identities and UAT evidence are controlling.
- Prefer OOP/SOLID/GRASP, useful GoF, TDD, KISS, YAGNI.
- Do not reveal hidden chain-of-thought.

Every substantive response starts with:

```text
Роль: независимый read-only Критик
Время обработки сообщения: ...
Время ответа: ... +03:00
```

and ends with:

```text
Вердикт: <краткое состояние по текущему действию>
Действие: <следующее конкретное действие>

Роль: независимый read-only Критик
Время обработки сообщения: ...
Время ответа: ... +03:00
```

`Вердикт` and `Действие` appear only once, at the end.

## Repositories

Context:
```text
alekseevvb/Context
branch main-way
root VoxFlux/Critic/
```

Production:
```text
alekseevvb/VoxFluxSTT
branch Genesis
```

## Closed history

P0.3:
```text
DONE / CLOSED
production speaker strategy = grouped_turns
```

PR #18 selected grouped_turns as production default.
PR #19 closed P0.3 in ROADMAP and added future owner-run contracts.
PR #20 repaired the ROADMAP Markdown debt-table row-break defect before deployment.

## P0.4 deployed implementation

PR #21 merged.

Current production:
```text
Genesis HEAD
78752386e747b90203b7b21bb43b222f8ae51b18

Genesis tree
cf9d41eaff0996d13727c2d6f053982c4c077bbe
```

Exact-head push CI:
```text
P0 Path Contract Gates
run #197
run_id 36244654908
event push
branch Genesis
head_sha 78752386e747b90203b7b21bb43b222f8ae51b18
completed / success

gates-py3.10 success
gates-py3.12 success
```

Fixed and regression-tested:
```text
R1 millisecond rollover / ,1000
FIXED

R2 negative / NaN / +inf / -inf timestamps
controlled ValueError
FIXED

R3 cue numbering independent of Segment.id
FIXED

empty collection
PRESERVED

overlap policy
PRESERVE
no silent clamp / shift / merge / drop
```

Formatter contract:
- reject non-finite or negative timestamps;
- `total_ms = int(round(seconds * 1000))`;
- decompose using `divmod`;
- enumerate cues with `enumerate(result.segments, start=1)`;
- do not sort by Segment.id;
- do not normalize overlaps.

## Canonical Drive deployment

```text
repository
alekseevvb/VoxFluxSTT

branch
Genesis

commit_sha
78752386e747b90203b7b21bb43b222f8ae51b18

commit_tree_sha
cf9d41eaff0996d13727c2d6f053982c4c077bbe

previous_commit_sha
77f7207c3732aac2cf2b52518ce896ecfd8c1909

tracked_file_count
165

tracked_tree_digest
74f535b152996eb2da47aceff611ffd1ee77b4cdea3661eaa888952aa4300867
```

CI in IDENTITY:
```text
run #197
run_id 36244654908
event push
completed / success
gates-py3.10 success
gates-py3.12 success
```

Sync summary:
```text
created 0
updated 3
unchanged 162
stale_trashed 0
class_b_preserved 489
generated_bytecode_trashed 0
```

Canonical IDENTITY Drive ID:
```text
1IP4qY0wsWLcw3CcjiOe6rDPBj6CG6rA8
```

Deployment gap:
```text
Git ↔ canonical Drive
IDENTICAL
```

## Current roadmap frontier

```text
P0.4 — SRT correctness
CURRENT / NOT CLOSED
```

Only remaining substantive gate:
```text
real SRT UAT
```

Do not start Q1/A2/R3/P4/B5 before P0.4 closure.

## PR #22 — reviewed UAT carrier

```text
PR #22
OPEN / DRAFT / NOT MERGED

base
Genesis@78752386e747b90203b7b21bb43b222f8ae51b18

head
b88d1a2729aa1582f93374f1748fac02f6ae86ca

head tree
cd65658ca70c2d4384c64c938304eb46a9a17e1f

changed files
1

production changes
0

DriveSync changes
0
```

Notebook:
```text
Infrastructure/Runtime/Colab/2026.09.26/
02. P0.4-B1-SRT-UAT-RUN-ME.ipynb
```

Reviewed notebook blob:
```text
c7394e8eb47c7cc5958f76a3e8945394a6e02b45
```

Carrier CI:
```text
run #199
run_id 36247904468
head b88d1a2729aa1582f93374f1748fac02f6ae86ca
pull_request
completed / success

Python 3.10
pytest / Ruff / mypy / SHA256 PASS

Python 3.12
pytest / Ruff / mypy / SHA256 PASS
```

Independent Critic verdict:
```text
READY_FOR_P0.4_OWNER_RUN
blocking findings: 0
non-blocking findings: 0
```

**Do not merge PR #22.**

If PR head or notebook blob moves, fresh review is required.

## Approved P0.4 UAT contract

Source delivery:
```text
CODE
GitHub exact checkout
→ /content/VoxFluxSTT

DATA / MODELS / EVIDENCE
Google Drive

EXECUTION
Colab /content
```

Expected production target:
```text
remote Genesis HEAD
78752386e747b90203b7b21bb43b222f8ae51b18

checked-out HEAD
78752386e747b90203b7b21bb43b222f8ae51b18

checked-out tree
cf9d41eaff0996d13727c2d6f053982c4c077bbe
```

Fail closed if live Genesis advanced before run.

Private Git access:
- Colab Secret `GITHUB_TOKEN`;
- tokenless remote URL;
- transient GIT_ASKPASS;
- GIT_TERMINAL_PROMPT=0;
- no token in remote URL/.git/config;
- remove askpass helper after acquisition.

Python provenance:
```text
VoxFlux.__file__
must be under
/content/VoxFluxSTT/Colab/Infrastructure/Libraries/VoxFlux/
```

Drive source imports forbidden.

Drive data-plane only:
```text
Colab/Input
Colab/Infrastructure/Models/Whisper
Colab/Infrastructure/Models/Pyannote
Infrastructure/Runtime/UAT/Data/P0_4
pipeline output
evidence
IDENTITY metadata
```

Input:
```text
AN-V01-part-001.mp3
SHA-256
eed08de95b0e8c6a7495990946413193097fc02ec7d92f476d7cbfb2f43176f0
```

Production config must use no explicit strategy override:
```text
diarization.enabled = true
diarization.strategy = grouped_turns
strategy_override_supplied = false
```

Formatter UAT invariants:
```text
cue_count == segment_count
cue_numbers == 1..N
timestamp syntax valid
timestamp_projection_exact == true
contains_comma_1000 == false
content_projection_exact == true
overlap_projection_exact == true
```

Timestamp projection:
```text
expected_start_ms = round(segment.start * 1000)
expected_end_ms   = round(segment.end * 1000)
```

Overlap reference must be calculated in rounded integer-millisecond domain.

Do not compare output byte-for-byte to historical P0.3 B1 SRT.

## Expected artifacts

Under:
```text
VoxFluxSTT/Infrastructure/Runtime/UAT/Data/P0_4/<RUN_ID>/
```

expect:
```text
P0.4-SRT-UAT-result-v001.json
P0.4-SRT-UAT-transcription-v001.json
P0.4-SRT-UAT-output-v001.srt
pipeline-output/
```

PASS evidence should contain:
- source_delivery=git_exact_checkout;
- remote Genesis head;
- checked-out HEAD/tree;
- VoxFlux.__file__;
- Drive identity metadata;
- input SHA-256;
- production strategy/no-override;
- cue/segment count;
- timestamp/content/overlap projection booleans;
- expected/actual overlap pairs;
- SRT/transcription hashes;
- runtime versions/timing;
- per-cue rows.

## Immediate next action

Owner has not yet run the notebook at this saved frontier.

Next:
```text
Open reviewed notebook from
uat/p04-real-srt-v001@b88d1a2729aa1582f93374f1748fac02f6ae86ca

select GPU runtime

ensure Colab Secrets:
GITHUB_TOKEN
HF_TOKEN

Runtime → Run all
```

Expected marker:
```text
[   PASS   ] P0.4 real SRT UAT complete
```

After run:
1. obtain full terminal output;
2. locate newest P0_4/<RUN_ID> on Drive;
3. read all three artifacts;
4. verify hashes/provenance/invariants;
5. final P0.4 closure review;
6. only then mark P0.4 DONE/CLOSED.

## Architectural follow-up

Accepted for P0.4 UAT:
```text
CODE → Git exact checkout
DATA → Drive
EXECUTION → /content
```

Do not migrate whole project inside P0.4.

```text
Git-direct for P0.4 UAT
ACCEPTED

DriveSync modification/removal now
DEFER

IDENTITY simplification now
DEFER

full source-delivery migration
DEFER to later roadmap
```

Likely future debt:
```text
D-032
Colab runtime source delivery duplicates Git through full-source Drive mirror.
target: P4.1
```

DriveSync remains compatibility mechanism until separately reviewed migration.

Future durable roadmap requirements already recorded:
```text
Q1.4
Owner-Run Notebook Contract v1

R3.5
Owner-Run Runtime Lifecycle Contract v1
```

Do not pull them into current P0.4 closure unless concrete blocker requires it.
