# 16-agent engineering workflow

A 15-page write-up ([multi-agent-workflow.pdf](multi-agent-workflow.pdf), March 17, 2026) of how I ran 16 Claude Code agents at once across 13 of my Rust, C++ and Python repositories on one Windows machine, and what it took to keep the hardware from becoming the bottleneck.

## What the document covers

- **Three agent roles.** 4 directed agents get specific tasks. 12 discovery agents each explore a repo, propose a numbered list of improvements and wait for approval. 1 audit agent summarizes everything that shipped at the end of the session.
- **The bottleneck.** Concurrent `cargo build` and git operations exhausted the consumer NVMe write cache after about an hour.
- **The fix, in six steps.** A 10 GB RAM disk (OSFMount), git settings that skip per-object fsync, Windows Defender exclusions, a shared `CARGO_TARGET_DIR` on the RAM disk, NTFS tweaks, and start/end session scripts.
- **Session routine.** One-time setup, the daily flow, and the prompt templates for each agent role.
- **Numbers.** Per-agent lines-of-code rates from one session and projections from them. These are self-reported by the agents, not independently measured, and the document labels them that way.
- **Risks and mitigations.** RAM disk data loss, agent-introduced bugs, conflicts, usage limits.

## What is not here

The document refers to a set of PowerShell scripts (`start-session.ps1`, `configure-defender.ps1`, `optimize-cargo.ps1` and others) under `scripts/agent-perf/`. Those scripts are not in this repository; only the PDF is.
