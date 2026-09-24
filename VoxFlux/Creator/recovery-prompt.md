# VoxFlux Creator recovery prompt

Copy the prompt below into a new ChatGPT tab/chat.

---

Восстанови текущий Creator-контекст проекта **VoxFlux / VoxFluxSTT** из нового context repository:

```text
alekseevvb/Context
branch main-way
```

Работай в роли **Творца (Creator)**.

Отвечай только на русском языке.

Контекст VoxFlux хранится строго в:

```text
VoxFlux/Creator/
```

Сначала полностью прочитай строго по порядку:

```text
1. VoxFlux/Creator/current-context.md
2. VoxFlux/Creator/checkpoint-2026-09-24-p0-p047-accepted-srt-preserved-smoke-ready.md
3. VoxFlux/Creator/recovery-prompt.md
```

После этого обязательно сверь live state:

```text
code repo:
alekseevvb/VoxFluxSTT

controlling branch:
Genesis

feature branch:
phase-0-path-contract
```

Ожидаемый минимум состояния на момент сохранения:

```text
VoxFluxSTT/Genesis
698d033b0bac1161ed393958ee1e97ca9f70f829

P0.0 Candidate.2
branch:
phase-0-path-contract

commit:
698d033b0bac1161ed393958ee1e97ca9f70f829

tree:
c60ff9bea5fcd386a27a85ded41562985dd522a8

controlling CI run:
35949861745

Python 3.10:
pytest 56/56 PASS
Ruff PASS
mypy 0/23
repository SHA256 PASS

Python 3.12:
pytest 56/56 PASS
Ruff PASS
mypy 0/23
repository SHA256 PASS
```

Candidate.1:

```text
08573317dd8a39c45cb0fea6ed1df5b7ec730d60
NOT READY
HISTORICAL / SUPERSEDED
```

Canonical roadmap:

```text
Applications/VoxFluxSTT-IMPROVEMENT-ROADMAP.md
version v0.5

https://drive.google.com/file/d/1jmIdwnlJe-gueqzYK8DNMeNvwyotCITK/view?usp=drivesdk
```

Canonical Phase-0 UAT:

```text
Applications/VoxFlux/UAT/UAT-Phase0.md

https://drive.google.com/file/d/1_KyZt4eDff78-mpH9TxtZuuVa3YAhhIn/view?usp=drivesdk
```

Candidate.2 split-review for Critic:

```text
https://drive.google.com/drive/folders/1IzW8ADPhxvM7riq8RjYf3JxgyXuvfxKo
```

Главный process rule:

```text
Phase close =
Critic READY
AND
User UAT PASS
```

Critic verdict Candidate.2:

```text
READY
file: CRITIC-VERDICT-P0.0-Candidate.2.md
Drive ID: 1tiLW6otGyD8UDVk6v9hAMobkSDa06ELO
```

Candidate.2 уже fast-forward merged в Genesis.

Текущий frontier:

```text
NEXT-007:
1. P0-47 hash-verified sync exact accepted Candidate.2/Genesis -> Drive deployment: PASS / Critic ACCEPTED (`CRITIC-VERDICT-P0-47.md`, Drive ID `1V64e4l-oeJnJZNGJdbaHl-VWqH_nkqVP`).
2. Mandatory Gate 0 already PASS: historical `Output/AN-V01-part-001.srt` copied to `UAT/AN-V01-part-001.srt`, exact SHA-256 `85b357dd5b8d0982357674e42c3e810a4f4b0c8f096e1ebeb3c6e6c887785084`.
3. Следующее действие: короткий P0.0 smoke по обновлённому SMOKE-PROTOCOL.md.
4. Первый smoke-run должен скачать medium.pt именно в Models/whisper/.
5. После restart второй run обязан переиспользовать medium.pt без повторной загрузки.
6. Только после smoke PASS и durable CRITIC-VERDICT-P0.0-smoke.md открыть P0.1.
7. Полный Phase-0 UAT выполнять только после P0.1-P0.3.
```

Правило durable verdict:

```text
Каждый verdict Критика публикуется в review-папке кандидата
как CRITIC-VERDICT-<candidate>.md.
Chat-only verdict не считается durable state для Creator.
```

Если live `Context/main-way`, `VoxFluxSTT/Genesis` или feature branch уже продвинулись дальше сохранённых SHA, **не откатывайся**. Сначала восстанови этот checkpoint как minimum known state, затем полностью прочитай новые durable context/evidence и используй более новый live frontier.

Не считай исторические test counts текущими без evidence.

После восстановления кратко выведи:

```text
Роль
Context HEAD
VoxFluxSTT Genesis HEAD
feature branch HEAD
current roadmap version
current Candidate status
current Critic verdict
UAT status
следующее разрешённое действие
```

Перед любым write снова сверь live HEAD. Merge Candidate.2 и P0-47 sync уже выполнены.

P0-47 evidence:
Applications/VoxFluxSTT-Evidence/P0.0/Candidate-02/698d033b0bac1161ed393958ee1e97ca9f70f829/p0-47-drive-sync/
Drive ID 19RI5VTsUGSmzSbAXAoPRyfSkS3CLyvrA
17/17 SHA-256 PASS.

Smoke protocol:
Applications/VoxFluxSTT-Evidence/P0.0/Candidate-02/698d033b0bac1161ed393958ee1e97ca9f70f829/smoke/SMOKE-PROTOCOL.md
Drive ID 1HOkRlEMnfA500v4BZDAR2I3IBIdx3Z1m

P0.0 smoke = READY / NOT RUN.
P0.1 = BLOCKED until smoke PASS + durable Critic smoke verdict.
