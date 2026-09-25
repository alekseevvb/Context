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
1. VoxFlux/Creator/Context/2026.25.09/04. 19-52-56. Environment PR14 green and P0.3 probe v004 pending/CONTEXT.md
2. VoxFlux/Creator/Context/2026.25.09/03. 04-06-03. Candidate1 expanded review context save/CONTEXT.md
3. VoxFlux/Creator/Context/README.md
4. VoxFlux/Creator/recovery-prompt.md
```

Checkpoint `04` является controlling snapshot. Более старые checkpoints используй только как историю там, где они не противоречат checkpoint `04`.

## Live code state to verify immediately

Repository:

```text
alekseevvb/VoxFluxSTT
```

Expected save-time Genesis:

```text
Genesis
1ac03afa5a040148a5842bff938a0ef478f70039
```

Expected active branch:

```text
phase-0-environment-runtime

HEAD
a3944a0bd3d9e851c250b354c022f1fdaaa03d9a

tree
07d28ab85ad3909db497c3b61f50caba8667c5a8
```

Expected PR:

```text
PR #14
refactor(runtime): centralize notebook lifecycle in Environment

state:
OPEN

mergeable:
true

merged:
false
```

Expected CI:

```text
run:
36162307065

gates-py3.10:
SUCCESS

gates-py3.12:
SUCCESS

formal-review-package:
SKIPPED
```

Если live GitHub продвинулся дальше, не откатывайся: прочитай более новый diff/commits и продолжай от live frontier.

## Active task 1 — Environment / PR #14

Owner explicitly required:
- package `VoxFlux.environment`;
- OOP/TDD;
- `Environment` as Facade/GRASP Controller;
- `RuntimeSession` Strategy;
- managed cell decorators through `@environment.step(...)`;
- persistent JSONL START/PASS/FAIL log;
- on FAIL: persist error + traceback and fsync **before** runtime shutdown;
- on success: shutdown through the same lifecycle;
- move path ownership into Environment as `Routes` + `RouteManager`;
- remove legacy `core.paths` and success-only Colab shutdown adapter;
- refactor working `Colab/VoxFlux.ipynb` to use Environment/Routes instead of scattered paths.

At save time PR #14 CI is green but **Critic verdict is not yet recorded**.

Next action:
1. verify live PR #14;
2. prepare normal review diff/transport for Critic;
3. obtain one short authoritative Critic verdict;
4. fix only concrete blockers;
5. do not merge before PASS/READY.

## Active task 2 — P0.3 speaker diarization

P0.2 is closed as:
`large-v3 + Silero VAD`.

Speaker labels and the lost short second-speaker reply around 03:09 moved to P0.3.

Canonical probe at save time:

```text
P0.3-SPEAKER-DIARIZATION-PROBE-RUN-ME.ipynb

Drive ID:
1Esx45wQ__fwauHB-ioAIwS4Iqhn5YWhG
```

This is **v004**.

v003 failed before diarization result because pyannote's MP3 chunk seek returned:

```text
158895 samples instead of expected 160000
```

v004 therefore:
- decodes the full MP3 once to 16 kHz mono PCM/WAV;
- passes full in-memory waveform + sample_rate to pyannote;
- performs full-file diarization;
- only then inspects 03:00–03:20;
- reports regular/exclusive detection around 03:09;
- checks Whisper/environment compatibility.

At save time v004 has **not yet produced a successful PROBE SUMMARY**.

Do not claim whether pyannote sees the woman until actual v004 output exists.

## Priority after PR #14

After Environment is reviewed/merged/synchronized, immediately return to P0.3. Do not start another unrelated infrastructure project.

