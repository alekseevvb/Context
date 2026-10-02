# VoxFluxSTT Creator checkpoint

role: Creator
timestamp_utc: 20261002-214900Z

main_repository: alekseevvb/VoxFluxSTT
main_branch: Genesis
main_head: a4efeb5a229cbd622d9bd9c438fe09a338cc27c8
main_tree: 383d53b2befa8390d95f96dc30eae02e21832408

pr_43:
  state: MERGED
  head: d901648b6a3319ad6d16bb3b0ab9e9afd0b90fd7
  critic_verdict: READY
  critic_tree: 383d53b2befa8390d95f96dc30eae02e21832408
  premerge_ci: PASS
  premerge_run: 37002867383
  merge_commit: a4efeb5a229cbd622d9bd9c438fe09a338cc27c8
  postmerge_run: 37069180454
  postmerge_status_at_save: QUEUED

current_gate: PR43_POST_MERGE_GENESIS_CI

buzz_reference_decision:
- Buzz is reference implementation, not dependency.
- Finish P0.4-F-02 Official Control first.
- If mismatch reaches timing/diarization, first isolate CTC forced alignment only versus Whisper word timestamps.
- Only after alignment evidence compare MSDD vs Sortformer on the same aligned words with manual speaker ground truth.
- Keep punctuation restoration out of the first alignment experiment.
- Faster-Whisper belongs to B5; stable-ts aligns with F01; model manager later infrastructure; Demucs optional preprocessing; queue/cancellation/hooks align with R3.

next_action:
- Wait for post-merge push CI run 37069180454 on Genesis a4efeb5a229cbd622d9bd9c438fe09a338cc27c8.
- After SUCCESS, Owner reruns Colab/GitHub_Синхронизация.ipynb.
- Verify Drive HEAD/tree and notebook identity.
- Then run Official Control notebook on GPU and analyze A/B/C evidence.
