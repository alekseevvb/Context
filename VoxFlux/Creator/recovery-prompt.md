# VoxFlux Creator recovery prompt

Восстанови текущий Creator-контекст проекта **VoxFlux / VoxFluxSTT**.

Работай в роли **Творца (Creator)**.

Отвечай только на русском языке.

Каждый substantive ответ должен содержать:

```text
Роль
Время обработки сообщения
Время ответа
```

При приближении context pressure заранее выдай копируемое предупреждение в fenced block со знаком `⚠️`.

## Context repository

```text
repo:
alekseevvb/Context

branch:
main-way

root:
VoxFlux/Creator/
```

Сначала полностью прочитай строго по порядку:

```text
1. VoxFlux/Creator/Context/2026.25.09/03. 04-06-03. Candidate1 expanded review context save/CONTEXT.md
2. VoxFlux/Creator/Context/2026.25.09/02. 04-05-28. Candidate1 expanded review publisher ready/CONTEXT.md
3. VoxFlux/Creator/Context/2026.25.09/01. 03-12-18. Phase0 Drive migration Candidate1 ready for Critic/CONTEXT.md
4. VoxFlux/Creator/Context/README.md
5. VoxFlux/Creator/recovery-prompt.md
```

Более старые checkpoints используй только как историю, если они не противоречат перечисленным выше controlling files.

## Code repository

После восстановления контекста обязательно сверь live state:

```text
repo:
alekseevvb/VoxFluxSTT

accepted Genesis:
698d033b0bac1161ed393958ee1e97ca9f70f829

current candidate branch:
phase-0-drive-migration-candidate-1

expected candidate:
82e4b32f53b16a5b86753662bf849c2d982b401c

expected tree:
9dbfcd4d25e0dfce82e7c5672f284208753263eb

PR:
#7

PR base:
Genesis / 698d033b0bac1161ed393958ee1e97ca9f70f829

PR state:
draft, open, NOT merged
```

Если live branch уже продвинулся, не откатывай автоматически. Сначала выясни причину и прочитай более новое durable evidence.

## Final structure verdict

```text
DRIVE-TARGET-LAYOUT v2
READY_WITH_CONDITIONS
```

Структура заморожена.

B1:
- новый Colab root уже должен быть в Git-кандидате:
  `/content/drive/MyDrive/VoxFluxSTT/Colab`;
- Whisper использует explicit `download_root`;
- VoxFlux не выставляет project-level `XDG_CACHE_HOME`.

B2:
- stale Class-A определяется через commit из предыдущего root `IDENTITY.json`;
- первая синхронизация без IDENTITY => stale set empty;
- invalid/unresolvable IDENTITY => FAIL CLOSED.

## Current Critic status

**Candidate.1 ещё не проверен. Вердикта нет.**

Блокировки:

```text
DRY-RUN-002:
BLOCKED

APPLY:
BLOCKED

PR #7 merge:
BLOCKED
```

Критик потребовал expanded review surface, потому что предыдущий 260 KiB ZIP не подходит для удобной проверки.

## Correct review range

Используй только:

```text
698d033b0bac1161ed393958ee1e97ca9f70f829
..
82e4b32f53b16a5b86753662bf849c2d982b401c
```

Ожидается:

```text
commit count:
15

changed path count:
43
```

Предыдущий review range от `3a2c38d...` является недостаточным для merge-review и не является controlling.

## Expanded review publisher

Owner-run notebook уже физически лежит на Google Drive:

```text
MyDrive/Applications/VoxFluxSTT/
Infrastructure/Runtime/Colab/2026.09.25/
02. PUBLISH-PHASE-0-DRIVE-MIGRATION-CANDIDATE-1-EXPANDED-REVIEW.ipynb
```

Drive ID:

```text
1n10Z6Zr2lPfTl_5hxP8NzYzUtiIP9ucp
```

Bytes:

```text
17364
```

SHA-256:

```text
3ae2550c57df6e923b8fddc9b72579077bf5dce04f6592a95111bf12e4733920
```

**На момент checkpoint этот notebook ещё не запускался.**

После первого успешного запуска он становится immutable. Любая правка после запуска идёт как notebook `03`.

## Expected expanded review output

```text
MyDrive/Applications/VoxFluxSTT/
Infrastructure/Environments/AI/Review/
P0-Migration/Candidate-001/
82e4b32f53b16a5b86753662bf849c2d982b401c/
Expanded-Review-Genesis-698d033/
```

Expected files:

```text
REVIEW.md
COMMITS.md
CHANGED-FILES.md
MANIFEST.json
SHA256SUMS
patches/
```

Topical patch groups:

```text
01 paths
02 notebooks
03 cache
04 AI-tree-and-UAT
05 context-retirement
06 tests
07 CI
08 documentation
```

Каждый generated review file должен быть меньше 40,000 bytes.

## Context retirement rationale

Критик потребовал объяснить лишний scope.

Controlling rationale:

```text
Creator durable private context
→ external repo alekseevvb/Context
→ VoxFlux/Creator/

Critic operational state
→ Infrastructure/Environments/AI/Critic/Context/ on Drive

Messages
→ Creator/Message/
→ Critic/Message/

Review evidence/verdicts
→ AI/Review/
```

Поэтому:

```text
make save-context
make load-context
Infrastructure/Runtime/Context/manage_context.py
```

не получают repository-side replacement.

Удалённые snapshots и tooling остаются восстанавливаемыми из Git history, включая Genesis и более ранние commits.

## Next action

Следующий шаг **только один**:

```text
Owner runs notebook 02.
```

После его выполнения:

1. прочитай published expanded-review files с Drive;
2. сверяй MANIFEST/SHA256SUMS;
3. подтверди 15 commits / 43 paths;
4. подтверди все patch groups;
5. создай immutable Creator->Critic message;
6. жди независимый Critic verdict.

До этого не запускать DRY-RUN-002, APPLY и не merge PR #7.


## Creator context snapshot convention

All Creator context saves now use:

```text
VoxFlux/Creator/Context/YYYY.DD.MM/NN. HH-mm-ss. Topic/CONTEXT.md
```

The timestamp is project-local Asia/Jerusalem time. `NN` is chronological within the date. Saved snapshots are immutable; every new save gets a new directory.
