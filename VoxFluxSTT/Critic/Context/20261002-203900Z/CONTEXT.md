# VoxFluxSTT Critic checkpoint

role: Critic
prompt_version: 1.8
timestamp_utc: 20261002-203900Z

topic: Buzz architecture research intake
source_repository: chidiwilliams/buzz
source_sha: 100589fd8c492869f2994b525e1092eda81e4e88

critic_assessment: ACCEPT_AS_RESEARCH_BACKLOG_WITH_SEQUENCING_CHANGES

## verified_buzz_facts

Buzz pyproject at source SHA:
- requires-python >=3.13,<3.14
- very broad dependency surface including PyQt6, stable-ts, faster-whisper, openai-whisper, transformers, NeMo ASR, Demucs-related dependencies, Vulkan, ONNX, etc.
- therefore do not add buzz-captions wholesale as a VoxFluxSTT dependency.

Speaker identification implementation:
buzz/widgets/transcription_viewer/speaker_identification_widget.py
- reconstructs full transcript text from existing transcript segments
- decodes original audio
- loads ctc_forced_aligner alignment model
- generates emissions
- aligns transcript tokens
- creates word timestamps via postprocess_results
- runs either MSDDDiarizer or SortformerDiarizer
- maps aligned words to speaker timestamps
- optionally restores punctuation
- realigns word-speaker mapping with punctuation
- derives sentence-speaker mapping

Faster Whisper implementation:
buzz/transcriber/whisper_file_transcriber.py
- resolves local model path
- constructs faster_whisper.WhisperModel
- uses faster_whisper.BatchedInferencePipeline
- optional reduced-memory compute_type:
  CPU int8
  GPU int8_float16
- otherwise compute_type default
- uses local cache/model path and batched inference.

stable-ts transcript resizer:
buzz/plugins/transcript_resizer/plugin.py
- operates after transcription when word-level timings exist
- uses stable_whisper.transcribe_any
- vad=False
- suppress_silence=False
- regroup configuration based on gap, punctuation and max length
- replaces transcript segments with regrouped subtitle-sized segments

Model management:
buzz/model_loader.py
- central model_root_dir
- BUZZ_MODEL_ROOT override
- DOWNLOAD_COMPLETE_MARKER=.buzz_complete
- snapshot completeness checks
- local_files_only checks
- retry/resume logic around model downloads

Plugin hooks verified:
buzz/plugins/manager.py
- check_skip
- before_transcription
- after_transcription
- on_complete

Demucs speech extraction:
buzz/file_transcriber_queue_worker.py
- separate process
- 300 second windows
- 10 second context
- overlap/context trimmed before writing
- streaming decode/encode rather than whole-file in-memory separation.

## licensing_verified

Buzz LICENSE: MIT.
MahmoudAshraf97/whisper-diarization LICENSE: BSD-2-Clause.
MahmoudAshraf97/ctc-forced-aligner LICENSE: BSD-2-Clause.
jianfch/stable-ts LICENSE: MIT.
facebookresearch/demucs LICENSE: MIT.

Caveat:
code license is not model-weight license. NeMo/HuggingFace diarization/alignment model weights and their terms must be reviewed separately before distribution/use decisions.
Direct code copying requires preservation of applicable notices.

## sequencing_decision

Do NOT interrupt or replace current P0.4-F-02 Official Control A/B/C experiment with Buzz-derived work.

Current F02 must first localize the mismatch:
A upstream-like Whisper
B current VoxFlux ASR policy
C current VoxFlux parser

Only if A/B/C are correct while production remains wrong, or evidence specifically localizes the defect to word timestamps / diarization / grouping / clip construction, Buzz-style alignment becomes a justified next diagnostic.

## recommended_buzz_alignment_spike_if_triggered

Do not start with a four-way end-to-end comparison that changes several variables simultaneously.

Stage 1 — CTC alignment only:
- same source audio/window/denominator
- freeze one exact transcript text
- compare current Whisper word timestamps vs CTC-forced-aligned word timestamps
- no diarizer change
- no punctuation restoration
- no subtitle regrouping
- measure coverage, monotonicity, boundary shifts, failed/unmapped words and confidence where available

Stage 2 — diarizer comparison:
- use the exact same CTC-aligned word set
- compare MSDD vs Sortformer
- punctuation restoration disabled initially
- compare against a small human-labelled speaker ground truth on the controlling denominator

Stage 3 — optional punctuation/sentence realignment:
- only after alignment and diarizer effects are independently understood.

This isolates variables and avoids attributing improvements to the wrong subsystem.

## roadmap_mapping

Forced alignment / alternative diarization:
- conditional diagnostic after P0.4-F-02 if evidence points to timing/speaker attribution boundary
- formal production integration belongs after clear provider/boundary design, not as ad hoc Pyannote growth

Faster-Whisper:
- valuable spike
- formal production provider belongs to B5 / stabilized provider boundary
- may be used earlier only as a narrow diagnostic if F02 shows an OpenAI Whisper inference-level problem
- do not lock current ITranscriber shape prematurely before A2/B5 architecture work

stable-ts:
- maps naturally to P0.4-F-01 subtitle readability / later SubtitleSegmenter
- do not evaluate "better subtitle grouping" before product readability contract is fixed
- ASR and subtitle segmentation should remain separate responsibilities

Model cache integrity:
- valuable design input for later infrastructure/package/model management work
- principle should be copied, not Buzz model_loader wholesale

Demucs:
- optional preprocessing provider only
- changes waveform and must be evidence-gated
- not part of current canonical baseline

Plugin hooks / queue / process isolation:
- useful later for R3/batch/local runtime
- not current P0.4 scope

## current_decision

Buzz is a good reference implementation source, not a dependency.

Most promising idea for current class of diarization problems:
CTC forced alignment before speaker attribution.

But it is not the next immediate action.
Next immediate action remains completing P0.4-F-02 Official Control and interpreting A/B/C evidence.
