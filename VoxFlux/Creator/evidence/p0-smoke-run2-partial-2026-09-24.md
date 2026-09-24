# VoxFlux P0.0 smoke — Run 2 partial evidence

**Date:** 2026-09-24
**Candidate / Genesis:** `698d033b0bac1161ed393958ee1e97ca9f70f829`
**Status:** RUN_2_PROCESSING_SUCCESS_CONFIRMED / NO_REDOWNLOAD_EVIDENCE_PENDING

## Operator-supplied output fragment

```text
Найдено файлов для обработки: 1

▶ Обработка: AN-V01-part-001.mp3 ...
[Pipeline] Старт инференса: AN-V01-part-001.mp3
✔ Готово! Сегментов: 129 | Субтитры: AN-V01-part-001.srt
📄 Превью текста: Всем привет! Меня должно быть слышно и видно. Сейчас буквально через минутку мы начинаем нашу первую лекцию. Всем привет! Меня должно быть слышно и ви...
```

## Drive readback after this run

```text
Output/AN-V01-part-001.srt
Drive ID: 1V9APCWMA4eYkrAD3Lm3-3rrRzfGJh8E6
size: 17518 bytes
modified: 2026-09-24T12:49:57.674Z

Infrastructure/Models/whisper/medium.pt
Drive ID: 1qoBWYKXW8rrWEQQCtQ4-gm9IVK1Ob4LC
size: 1528008539 bytes
modified: 2026-09-24T12:36:18.095Z
```

This confirms:
- the second processing pass reached successful SRT generation;
- the result differs from Run 1 (129 vs 138 segments), which is acceptable;
- the existing `medium.pt` object remained present.

This fragment does NOT by itself prove the required no-redownload criterion,
because the Run 2 pipeline-initialization lines / absence of a model download
progress bar were not included.

Formal Run 2 PASS remains pending the missing initialization evidence.
