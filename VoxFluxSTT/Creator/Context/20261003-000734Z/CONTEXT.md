# VoxFluxSTT Creator checkpoint

role: Creator
timestamp_utc: 20261003-000734Z

main_repository: alekseevvb/VoxFluxSTT
main_branch: Genesis
main_head: a4efeb5a229cbd622d9bd9c438fe09a338cc27c8
main_tree: 383d53b2befa8390d95f96dc30eae02e21832408
main_ci_status: PASS — push run 37069180454

owner_decision:
- Before resuming P0.4-F-02, promote the nearly completed VoxFluxII refactor into the canonical VoxFlux package.
- VoxFluxII becomes VoxFlux; use full regression to validate the consolidated package.

migration_pr:
  number: 44
  branch: refactor/promote-voxfluxii-to-voxflux
  state: OPEN
  draft: true
  mergeable: true
  base: a4efeb5a229cbd622d9bd9c438fe09a338cc27c8
  head: 61dcb1d5980f8a3876981ee90d6ccee1df3a6d91
  tree: 9b8c4298b75b752b1301f0e466e9e252cfba3371
  ahead_by: 2
  behind_by: 0

tdd:
  red_commit: 95fd90ca7c8a04779ba1823ab9ad1fd9bf476c40
  red_run: 37080149579
  red_result: FAILURE on Python 3.10 and 3.12 at Pytest after canonical install PASS
  implementation_commit: 61dcb1d5980f8a3876981ee90d6ccee1df3a6d91
  green_run: 37080401870
  green_result: SUCCESS
  python_3_10: 197 passed; Ruff PASS; mypy PASS 56 source files; SHA256 PASS
  python_3_12: 197 passed; Ruff PASS; mypy PASS 56 source files; SHA256 PASS

migration_scope:
- VoxFluxII/logger promoted byte-for-byte to VoxFlux/logger.
- VoxFluxII/routes promoted byte-for-byte to VoxFlux/routes.
- VoxFluxII runtime package removed.
- Four VoxFluxII-specific tests renamed and imports changed to VoxFlux.
- VoxFlux.Route now exposes the accepted immutable Route topology.
- Existing VoxFlux.Routes and RouteManager remain environment-owned for current notebook/runtime compatibility.
- Existing VoxFlux core, pipeline, providers, Environment implementation are unchanged.
- setuptools package discovery is now canonical VoxFlux only.
- Makefile quality scope checks the complete canonical VoxFlux tree.
- PATH-15 ownership and TODO references updated to canonical VoxFlux paths.

critic_review:
  requested: true
  pr_comment_id: 5963432610
  exact_sha: 61dcb1d5980f8a3876981ee90d6ccee1df3a6d91
  verdict: PENDING

current_gate: PR44_CRITIC_REVIEW

merge_policy:
- Do not merge PR #44 without exact-SHA Critic READY.
- Any HEAD movement makes a verdict for the prior SHA stale.

p04_f02:
- PR #43 notebook newline fix is already merged into Genesis a4efeb5a229cbd622d9bd9c438fe09a338cc27c8.
- Do not run stale Drive notebook before PR #44 decision and subsequent canonical Drive synchronization.
- After PR #44 merge + post-merge CI GREEN: rerun GitHub_Синхронизация.ipynb, verify Drive HEAD/tree and notebook byte identity, then execute Official Control on GPU.

buzz_reference_decision:
- Buzz remains a reference implementation only.
- Finish P0.4-F-02 A/B/C localization before any Buzz-style forced-alignment experiment.
- If timing/diarization becomes the first mismatch, isolate CTC forced alignment before MSDD/Sortformer or punctuation changes.
