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
2. VoxFlux/Creator/checkpoint-2026-09-24-p0-candidate2-awaiting-critic.md
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
bdc0c5683816fdcd45e696e24feba1e6612412dc

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

Текущий frontier:

```text
NEXT-006:
получить независимый verdict Критика по P0.0 Candidate.2.
```

До verdict `Critic READY` строго запрещено:

```text
- merge Candidate.2 в Genesis;
- P0-47 Drive deployment sync;
- Colab smoke;
- переход к P0.1.
```

После `Critic READY` порядок строго такой:

```text
1. P0-47: hash-verified sync exact accepted Candidate.2 -> Drive deployment.
2. Короткий behavior-preserving P0.0 smoke по UAT-Phase0.md.
3. Только после smoke PASS перейти к P0.1.
4. Полный Phase-0 UAT выполнять только после P0.1-P0.3.
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

И остановись перед любым write/merge/sync, если новый verdict Критика ещё не передан.
