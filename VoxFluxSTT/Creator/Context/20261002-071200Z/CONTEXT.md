# VoxFluxSTT Creator checkpoint

role: Creator
prompt_version: 1.8
timestamp_utc: 20261002-071200Z

main_repository: alekseevvb/VoxFluxSTT
main_branch: Genesis
main_head: 51afb9d68ea5835eb9b3f25af15185021145397b
main_tree: e27f50c7efa48f8221cc8fcaf1190be34347db1b
main_ci_status: PASS

main_sync: DRIVE_STALE_BLOCKER_FOUND
work_frontier: FEATURE_BRANCH
relevant_branch: fix/drive-sync-protected-tree
relevant_branch_head: e1982179a6d0232363c09113fb9629696514053e
pr: 40
pr_state: OPEN / DRAFT
critic_verdict: NONE for PR #40 current SHA

context_repository: alekseevvb/Context
context_root: VoxFluxSTT/Creator
context_head_before_save: 1ca2ea47c900d25443fcc1a0e879bf16d18f2670

## Owner request
Actualize Google Drive deployment, verify operability, then create a narrow control notebook based on official Colab/VoxFlux.ipynb for P0.4-F-02.

## Drive live state
Canonical root: MyDrive/VoxFluxSTT
Drive folder id: 1lF0--z-wb0gi5xvEgdTB02c2vvafB0-r
Drive Git HEAD ref: refs/heads/Genesis
Drive Genesis SHA: 2f42983c1f715ebfa4685f8e38d288dd8dd59170
Current GitHub Genesis SHA: 51afb9d68ea5835eb9b3f25af15185021145397b
Drive source working tree is stale.

Old IDENTITY.json is legacy mirror metadata and is not authoritative for Git-native sync.

Drive launcher Colab/GitHub_Синхронизация.ipynb raw blob matches current Genesis blob:
ee17b47bdd0b45b9d8ec4458bc71d76b485d0cc2

Drive official Colab/VoxFlux.ipynb raw blob matches current Genesis blob:
050213e5b1a147d516bd25e301937ba5a464eca4

## Runtime overlay
Input AN-V01-part-001.mp3 exists on Drive, size 4793040 bytes.
F02 expected SHA256: eed08de95b0e8c6a7495990946413193097fc02ec7d92f476d7cbfb2f43176f0
Whisper cache contains large-v3.pt, medium.pt, small.pt.
Pyannote cache contains models--pyannote--speaker-diarization-community-1.
No F02_Diagnostic evidence folder currently exists.

## Synchronization blocker
The GitRepositorySynchronizer at Drive HEAD 2f42983c and current Genesis is byte-identical.
Protected roots include Infrastructure/Warehouse/Archive/.
Current Genesis has 28 protected tracked files.
Exactly one violates the existing allowlist:
Infrastructure/Warehouse/Archive/routes.py

This file did not exist at Drive HEAD, was introduced by commit 523d0046, has no production references, and makes the current Drive launcher fail closed before checkout.

## PR #40
Branch: fix/drive-sync-protected-tree
Base Genesis: 51afb9d68ea5835eb9b3f25af15185021145397b

RED commit: aabca3d21bf1b05552e43822858aaf1128661d1b
RED run: 36976576217
Python 3.10: 1 failed / 191 passed
Python 3.12: 1 failed / 191 passed
Sole failure: test_current_git_tree_respects_protected_tracked_allowlist
Exact unexpected path: Infrastructure/Warehouse/Archive/routes.py

Implementation commit: e1982179a6d0232363c09113fb9629696514053e
Candidate tree: 256a441f2b73253168f6313abc4246c6046a5fb6
Implementation removes only Infrastructure/Warehouse/Archive/routes.py and does not widen the allowlist.

GREEN run: 36976717524
Python 3.10: canonical install PASS, 192 passed, Ruff PASS, mypy PASS 56 files, SHA256 PASS
Python 3.12: canonical install PASS, 192 passed, Ruff PASS, mypy PASS 56 files, SHA256 PASS

PR #40: OPEN / DRAFT / mergeable
2 commits, 2 changed paths, behind_by 0.

## Drive gate
Do not run Drive sync before PR #40 receives exact-SHA Critic READY and is merged.
After merge and post-merge CI, Owner runs Colab/GitHub_Синхронизация.ipynb with Run all using Colab GITHUB_TOKEN.
Creator then verifies Drive refs/heads/Genesis equals new Genesis and runtime overlays are preserved.

## P0.4-F-02 next diagnostic
Official notebook basis: Colab/VoxFlux.ipynb.
Existing authorized primary diagnostic: Infrastructure/Runtime/Colab/2026.09.27/05. 2026.09.27 01-40. P0.4 F02 Narrow Diagnostic. Colab.ipynb
Target denominator: 184-194 s.

After Drive PASS create a separate narrow control notebook using official bootstrap/model-cache conventions, not a full pipeline rerun.
Planned direct-control window: approximately 174-204 s with comparison restricted to 184-194 s.
Compare direct Whisper large-v3 without external VAD/grouping against current VoxFlux ASR kwargs and production F02 B1/B2 evidence.

Interpretation:
- direct Whisper correct + production wrong => VAD/diarization/grouping/clip boundary
- upstream-like direct correct + VoxFlux kwargs wrong => ASR config policy
- raw model correct + provider parse wrong => provider/parser
- both direct modes wrong => Whisper/model/audio upstream behavior

## current task
Await independent Critic review of PR #40 exact SHA e1982179a6d0232363c09113fb9629696514053e.

## important decisions
Do not allowlist accidental Archive/routes.py; remove it.
Keep a regression guard against actual tracked protected paths.
Do not touch accepted VoxFluxII/routes architecture.
Do not run full pipeline for F02 localization.
