# VoxFluxSTT Critic checkpoint

role: Critic
prompt_version: 1.8
timestamp_utc: 20261001-183200Z

main_repository: alekseevvb/VoxFluxSTT
main_branch: Genesis
main_head: 2f42983c1f715ebfa4685f8e38d288dd8dd59170

work_frontier: FEATURE_BRANCH
relevant_branch: VoxFlux_Genesis
relevant_branch_head: b13726aef11c6ed81ac43adce85cabe8692d8508
relevant_branch_tree: 15d4d66e2496ef16976de9beed77cefa1414455d

pr: 36
pr_state: OPEN / DRAFT
pr_head: b13726aef11c6ed81ac43adce85cabe8692d8508
merge_performed: NO

critic_verdict: NOT_READY
verdict_sha: b13726aef11c6ed81ac43adce85cabe8692d8508

context_repository: alekseevvb/Context
context_root: VoxFluxSTT/Critic
context_head_before_save: 0f6ea6ab499c543f5f5f27887529daf50790b847

## reviewed_range

2b369cdfdd0763d18afd73e4f01395d4f3b8f712...b13726aef11c6ed81ac43adce85cabe8692d8508

Commits:
- e0043da1c1d0b619c51c57b5626d100378705242
  test(routes): define simplified architecture contracts
- b13726aef11c6ed81ac43adce85cabe8692d8508
  refactor(routes): simplify topology and runtime responsibilities

## confirmed_good

- Node removed.
- RoutePath removed; pathlib LSP violation closed.
- Route.root and hidden _DEFAULT_ROOT removed.
- Route now immutable relative Enum topology.
- ASR model/cache state removed from per-media Routes.
- RoutesFactory.create requires explicit created_at and does no filesystem reads.
- RoutesDiscovery is decoupled from RoutesFactory and yields Path.
- SUPPORTED_AUDIO_EXTENSIONS is frozenset.
- RED run 36904366153 genuinely failed 12 new architecture tests on Python 3.10 and 3.12.
- The architecture RED test file was not changed in implementation commit.
- Exact-head GREEN run 36905101771:
  - Python 3.10: 188 passed; Ruff PASS; mypy PASS 60 source files; SHA256 PASS.
  - Python 3.12: 188 passed; Ruff PASS; mypy PASS 60 source files; SHA256 PASS.

## blocking_findings

B1 — hidden process-CWD dependency remains in explicit root contracts.
RoutesFactory(root) calls os.path.abspath(root), so a relative root is silently bound to current working directory. Same constructor argument can resolve differently under different CWD values. This contradicts deterministic/explicit dependency claims and is inconsistent with Routes validation, which requires an absolute root.
RoutesDiscovery(input_dir) likewise resolves a relative input_dir against CWD.
Required:
- fail-fast on non-absolute project root;
- make an explicit policy for input_dir, preferably absolute-only;
- RED tests for relative root rejection and CWD-independence.

B2 — recursive discovery can recurse forever on an internal symlink cycle.
RoutesDiscovery resolves directory entries and recursively follows resolved directories. Containment checks reject only targets outside Input. A symlink such as Input/A/back -> Input remains within Input and causes unbounded recursion.
Required:
- explicit symlink traversal policy;
- simplest fail-closed option: do not recurse through directory symlinks;
- alternatively track visited canonical directories;
- RED tests for self/ancestor cycle and duplicate traversal via internal directory symlink.

B3 — unapproved public/canonical Route contract regression.
At previous candidate Route contained:
- RADIX
- INFRASTRUCTURE
- PARAKEET
- TitleCase aliases: Route.Input, Route.Output, Route.Whisper, etc.
These were documented/intended API, not classified as defects.
The new simplified Route omits RADIX, INFRASTRUCTURE, PARAKEET and all TitleCase aliases.
This violates the project's characterization rule unless explicitly approved by Owner as a breaking migration.
The new architecture test hard-codes the reduced set, so TDD currently encodes an unapproved regression.
Required:
- add characterization tests for canonical Route surface;
- preserve needed canonical entries/API in the immutable design, or obtain explicit Owner migration decision before removal.

## non_blocking_followups

- Consider requiring timezone-aware created_at if log timestamps have timezone semantics.
- _route_path helper is duplicated in routes.py and factory.py; small DRY cleanup is possible but do not create a resolver abstraction unless justified.
- Routes(root=..., ...) uses root only as construction/validation context and does not store it; acceptable for now but monitor clarity.
- loguru/rich canonical pyproject dependency declaration remains a separate follow-up.
- Output collision policy remains intentionally outside RoutesDiscovery at future batch/orchestration boundary.

## pr_transport

PR #36 comment:
CRITIC VERDICT: NOT_READY @ b13726aef11c6ed81ac43adce85cabe8692d8508

comment_id: 5938064455

## next_action

Do not merge PR #36.

Creator should make one narrow TDD closure:
1. RED for B1 absolute explicit roots / CWD independence.
2. RED for B2 symlink cycles.
3. RED characterization for B3 canonical Route surface.
4. Minimal implementation.
5. Exact-head full GREEN.
6. Full architecture re-review by Critic.
