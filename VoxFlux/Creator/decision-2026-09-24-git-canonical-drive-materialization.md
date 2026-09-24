# VoxFlux process decision — Git canonical, Drive materialized by Colab

**Date:** 2026-09-24
**Role:** Creator
**Status:** ACTIVE / DRIVE_RESTRUCTURE_PENDING_DESIGN_APPROVAL

## Problem

Direct Creator writes to Google Drive are slow, error-prone, and repeatedly hit
connector limitations for ZIP review packages and large model files.

The current Drive project surface is fragmented across:
- Applications/VoxFlux/
- Applications/VoxFluxSTT-Evidence/
- Applications/VoxFluxSTT-IMPROVEMENT-ROADMAP.md
- multiple VoxFlux-backup-pre-P0-47-* folders

This makes repository state, runtime state, review evidence, and backups diverge.

## New canonical-source rule

GitHub becomes the only canonical source for:
- source code;
- notebooks;
- review manifests;
- transfer manifests;
- Critic/Creator handoff text intended to be materialized on Drive;
- deterministic tooling that builds Drive review surfaces.

Google Drive becomes a materialized runtime/evidence surface.

## New transfer pipeline

When data must be passed to the Critic:

1. Creator prepares everything in Git.
2. Creator creates a dedicated transfer Colab notebook in Git.
3. The notebook downloads the exact commit/artifacts/files from GitHub.
4. The notebook writes them to the approved Drive target paths.
5. The notebook computes and writes SHA-256 / manifest evidence.
6. The user runs the notebook in Colab.
7. Creator reads the resulting Drive surface and verifies it.
8. Only then is the Critic given the handoff message/link.

No package is considered published merely because Creator attempted a connector
upload.

## Transfer notebook requirements

Every transfer notebook must:
- pin the exact repository and commit SHA;
- fail closed if the fetched commit identity is different;
- create only approved target directories;
- never overwrite runtime/model/input/output state unless explicitly authorized;
- write MANIFEST.json and SHA256SUMS;
- print a concise final PASS/FAIL summary;
- be idempotent or explicitly refuse a conflicting pre-existing target;
- preserve historical review packages rather than mutating them;
- support plain split-review publication for Critic channel limitations.

## Current Critic state

The Critic status file for historical P0.1 Candidate.1 exists:

CRITIC-STATUS-P0.1-Candidate.1.md

Drive ID:
1gN3NCwPlSAqqPFxGN5PbQomFpecH9wDV

Candidate.1 is already superseded by Candidate.2 and remains historical.
The split-review requirement must therefore be applied to current Candidate.2,
not by reviving Candidate.1.

## Drive restructure guard

Do not move/delete/rename the existing Drive project tree until the target layout
is explicitly discussed and approved.

First design the desired Drive tree, mapping every current surface to a target.
Only after approval create a migration notebook in Git and execute the migration
through Colab with pre/post inventory and hashes.
