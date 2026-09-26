# VoxFlux / VoxFluxSTT — independent Critic context

Saved at: **2026-09-26 17:53 +03:00 (Asia/Jerusalem)**

## Canonical save layout

This snapshot follows the controlling Critic context layout:

```text
VoxFlux/Critic/Context/
└── YYYY.MM.DD/
    └── HH-mm/
        ├── CONTEXT.md
        └── RECOVERY-PROMPT.md
```

For this save:

```text
VoxFlux/Critic/Context/2026.09.26/17-53/
```

Each future save must create a new `HH-mm` directory under the current `YYYY.MM.DD` directory. Do not overwrite an older save.

## Role and response contract

Work as **independent read-only Critic**.

- Reply only in Russian for substantive project work.
- Production repository `alekseevvb/VoxFluxSTT` is read-only.
- One roadmap item at a time.
- Evidence-driven and fail-closed.
- Do not broaden scope silently.
- Production changes follow: problem/current behavior → proposal → discussion → Creator implementation/tests → Critic review.
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

Context repository:

```text
alekseevvb/Context
branch main-way
root VoxFlux/Critic/
```

Production repository:

```text
alekseevvb/VoxFluxSTT
branch Genesis
```

## Closed history relevant to current frontier

### P0.3

```text
P0.3
DONE / CLOSED

production speaker strategy
grouped_turns
```

PR #18 selected `grouped_turns` as production default.

PR #19 closed P0.3 in ROADMAP and added future owner-run contracts.

PR #20 repaired the ROADMAP Markdown debt-table row-break defect before canonical Drive deployment.

## P0.4 deployed implementation

PR #21 is merged.

Current production Genesis:

```text
HEAD
78752386e747b90203b7b21bb43b222f8ae51b18

tree
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

P0.4 implementation status:

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
- compute `total_ms = int(round(seconds * 1000))`;
- decompose with `divmod`;
- enumerate cues with `enumerate(result.segments, start=1)`;
- do not sort by Segment.id;
- do not normalize overlaps.

## Canonical Drive deployment

Canonical Drive is synchronized to current production Genesis.

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

CI inside canonical IDENTITY:

```text
run #197
run_id 36244654908
event push
completed / success
gates-py3.10 success
gates-py3.12 success
```

Last sync summary:

```text
created                      0
updated                      3
unchanged                  162
stale_trashed                0
class_b_preserved          489
generated_bytecode_trashed   0
```

Canonical `IDENTITY.json` Drive file ID:

```text
1IP4qY0wsWLcw3CcjiOe6rDPBj6CG6rA8
```

Deployment invariant:

```text
Git ↔ canonical Drive
IDENTICAL
```

## Current controlling roadmap item

```text
P0.4 — SRT correctness
CURRENT / NOT CLOSED
```

Production implementation is fixed, merged, deployed and tested.

Only remaining substantive closure gate:

```text
real SRT UAT
```

Do not start Q1/A2/R3/P4/B5 before P0.4 closure.

## PR #22 — reviewed Owner-run UAT carrier

PR #22 is an Owner-run carrier only.

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

Reviewed notebook:

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
P0 Path Contract Gates
run #199
run_id 36247904468
head b88d1a2729aa1582f93374f1748fac02f6ae86ca
event pull_request
completed / success

Python 3.10
pytest / Ruff / mypy / SHA256 PASS

Python 3.12
pytest / Ruff / mypy / SHA256 PASS
```

Latest independent Critic verdict:

```text
READY_FOR_P0.4_OWNER_RUN

blocking findings
0

non-blocking findings
0
```

**Do not merge PR #22 into Genesis.**

If PR head or notebook blob moves, the review is stale and must be repeated.

## Approved P0.4 UAT execution contract

Source-delivery boundary:

```text
CODE
GitHub exact checkout
→ /content/VoxFluxSTT

DATA / MODELS / EVIDENCE
Google Drive

EXECUTION
Colab /content
```

### Git source provenance

Expected production target:

```text
remote Genesis HEAD
78752386e747b90203b7b21bb43b222f8ae51b18

checked-out HEAD
78752386e747b90203b7b21bb43b222f8ae51b18

checked-out tree
cf9d41eaff0996d13727c2d6f053982c4c077bbe
```

Notebook must fail closed if live Genesis advances before the run.

Private Git access:
- Colab Secret `GITHUB_TOKEN`;
- tokenless remote URL `https://github.com/alekseevvb/VoxFluxSTT.git`;
- transient `GIT_ASKPASS`;
- `GIT_TERMINAL_PROMPT=0`;
- token is not stored in remote URL or `.git/config`;
- askpass helper is deleted after Git acquisition.

Python import provenance:

```text
VoxFlux.__file__
must be under
/content/VoxFluxSTT/Colab/Infrastructure/Libraries/VoxFlux/
```

Drive source imports are forbidden.

### Drive is data-plane only

Use Drive for:

```text
Colab/Input
Colab/Infrastructure/Models/Whisper
Colab/Infrastructure/Models/Pyannote
Infrastructure/Runtime/UAT/Data/P0_4
pipeline output
evidence
IDENTITY metadata
```

### Exact real input

```text
AN-V01-part-001.mp3

SHA-256
eed08de95b0e8c6a7495990946413193097fc02ec7d92f476d7cbfb2f43176f0
```

### Production configuration

The UAT must not supply an explicit speaker strategy override.

Required:

```text
diarization.enabled
true

diarization.strategy
grouped_turns

strategy_override_supplied
false
```

### Formatter projection invariants

Generated SRT is compared one-to-one with the `TranscriptionResult` from the same production run.

Required PASS:

```text
cue_count == segment_count
cue_numbers == 1..N
timestamp syntax valid
timestamp_projection_exact == true
contains_comma_1000 == false
content_projection_exact == true
overlap_projection_exact == true
```

Reference timestamp domain:

```text
expected_start_ms = round(segment.start * 1000)
expected_end_ms   = round(segment.end * 1000)
```

Expected overlap geometry must be calculated from the rounded integer-millisecond coordinates and compared with actual SRT overlap geometry.

Do not compare UAT output byte-for-byte with historical P0.3 B1 SRT.

## Expected UAT artifacts

After successful Owner run:

```text
VoxFluxSTT/Infrastructure/Runtime/UAT/Data/P0_4/<RUN_ID>/
```

Expected artifacts:

```text
P0.4-SRT-UAT-result-v001.json
P0.4-SRT-UAT-transcription-v001.json
P0.4-SRT-UAT-output-v001.srt
pipeline-output/
```

PASS result JSON should contain:
- source_delivery = git_exact_checkout;
- remote Genesis head;
- checked-out HEAD/tree;
- VoxFlux module path;
- Drive identity metadata;
- input SHA-256;
- production strategy/no override;
- segment/cue count;
- timestamp/content/overlap projection booleans;
- expected/actual overlap pairs;
- SRT and transcription hashes;
- runtime versions/timing;
- per-cue projection rows.

## Immediate next action

Owner has not yet run the notebook at this saved frontier.

Before Owner-run independently verify:

```text
Genesis
==
78752386e747b90203b7b21bb43b222f8ae51b18

PR #22 head
==
b88d1a2729aa1582f93374f1748fac02f6ae86ca

notebook blob
==
c7394e8eb47c7cc5958f76a3e8945394a6e02b45
```

If unchanged:

```text
open reviewed notebook from
uat/p04-real-srt-v001@b88d1a2729aa1582f93374f1748fac02f6ae86ca

select GPU runtime

Colab Secrets:
GITHUB_TOKEN
HF_TOKEN

Runtime → Run all
```

Expected terminal marker:

```text
[   PASS   ] P0.4 real SRT UAT complete
```

After run:
1. obtain full terminal output;
2. locate newest `P0_4/<RUN_ID>` on Drive;
3. read all three durable artifacts;
4. verify hashes/provenance/invariants;
5. perform final P0.4 closure review;
6. only after PASS mark `P0.4 DONE / CLOSED`.

## Architectural follow-up discovered during P0.4

Accepted for P0.4 UAT:

```text
CODE → Git exact checkout
DATA → Drive
EXECUTION → /content
```

Do not migrate the whole project inside P0.4.

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

DriveSync remains a compatibility mechanism until a separately reviewed migration.

Future durable roadmap requirements already recorded:

```text
Q1.4
Owner-Run Notebook Contract v1

R3.5
Owner-Run Runtime Lifecycle Contract v1
```

Do not pull either into current P0.4 closure unless a concrete blocker requires it.
