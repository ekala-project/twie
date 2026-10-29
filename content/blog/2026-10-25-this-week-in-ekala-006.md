+++
title = "This Week in Ekala #6"
date = 2026-10-25
description = "Cache server overhaul, KDE packaging, new Bun builder, AI-powered CI gates, and self-hosted forge support"

[taxonomies]
tags = ["progress", "corepkgs", "ekapkgs", "ekapkgs-cli", "eka-ci", "python-packages"]
+++

Welcome to This Week in Ekala #6! Here's what happened this week.

<!-- more -->

## Highlights

- The ekapkgs-cli cache server received a massive performance overhaul — streaming NAR serving, SQLite-backed search indexes, tiered zstd compression, and a dedicated compression worker pool
- KDE Frameworks and KDE Gear landed in ekapkgs with ~300 packages in a new `kde/` directory
- Default ecosystem versions bumped across the board: Linux kernel 6.18, Ruby 4.0, Node.js 26, Erlang/OTP 28
- eka-ci gained AI agent skill gates that run model evaluations against PR diffs

## corepkgs

2 packages were updated in a batch sweep and best-practices cleanups were applied treewide.

- [buildBunPackage: init](https://github.com/ekala-project/corepkgs/pull/240)<br>
  Introduced a dedicated builder for Bun packages, enabling first-class support for the growing Bun ecosystem in ekapkgs.
- [Bump default ecosystem versions](https://github.com/ekala-project/corepkgs/pull/242)<br>
  Major default version bumps: Linux kernel 6.12 → 6.18 LTS, Ruby 3.4 → 4.0, Node.js 24 → 26 LTS, and Erlang/OTP 27 → 28. Also adds new variants for LLVM 22, Perl 5.44, and PHP 8.5.

## ekapkgs

86 packages were added across Rust (34), prebuilt binaries (26), Bun (19), Python (3), and database packages (4). Contributed by [@ic4-y](https://github.com/ic4-y) and [@jonringer](https://github.com/jonringer).

- [postgresql: wire extension dir into postgresqlPackages](https://github.com/ekala-project/ekapkgs/pull/81)<br>
  Wired a dedicated `postgresql/pkgs/` directory into the `postgresqlPackages` scope, establishing the scaffolding for PostgreSQL extension packages. Contributed by [@ic4-y](https://github.com/ic4-y).
- [Mass additions 2 (KDE)](https://github.com/ekala-project/ekapkgs/pull/132)<br>
  Added ~300 KDE Frameworks and KDE Gear packages in a new `kde/` directory, establishing foundational KDE packaging infrastructure for ekapkgs.

## ekapkgs-cli

Bug fixes and observability improvements were made alongside the major changes below.

- [Optimize IO](https://github.com/ekala-project/ekapkgs-cli/pull/21)<br>
  Reader lock optimizations to reduce SQLite contention and improve concurrent read throughput in the cache server.
- [Reduce memory use](https://github.com/ekala-project/ekapkgs-cli/pull/22)<br>
  Switched to streaming throughout NAR serving and compression paths, eliminating large in-memory allocations.
- [search: migrate indexes from json.zst to SQLite](https://github.com/ekala-project/ekapkgs-cli/pull/23)<br>
  Replaced in-memory zstd-compressed JSON with a single `search.db` SQLite database for package, option, and file search indexes.
- [serve: replace storage_format with nullable zstd_level column](https://github.com/ekala-project/ekapkgs-cli/pull/24)<br>
  Unified chunk compression state into a single nullable column with background recompression from zstd-1 to zstd-9.
- [serve: performance improvements across storage, CPU, and memory](https://github.com/ekala-project/ekapkgs-cli/pull/25)<br>
  11 targeted improvements including SQLite tuning, bloom filters, LRU caches, zero-copy reads, and parallel delta computation.
- [template: add --forge flag for self-hosted GitHub/GitLab instances](https://github.com/ekala-project/ekapkgs-cli/pull/28)<br>
  New `--forge` option enables template generation from self-hosted GitHub and GitLab instances, with GitLab API metadata fetching and automatic tag prefix detection.
- [serve: dedicated zstd-9 worker pool for chunk compression](https://github.com/ekala-project/ekapkgs-cli/pull/29)<br>
  Added a dedicated rayon thread pool for zstd-9 compression with zstd-1 overflow fallback to keep latency bounded under load.

## eka-ci

- [feat: AI agent skill gates for CI](https://github.com/ekala-project/eka-ci/pull/217)<br>
  Introduced a new "skills" gate type that runs AI model evaluations against PR diffs. Skills are defined with a prompt file and optional model ID; providers are configured server-side to keep API keys out of repository config. The model returns structured pass/fail verdicts mapped to GitHub check runs.

## python-packages

Broad fix sweep applied across hundreds of Python package definitions, plus a new best-practices AI skill definition and GitHub Actions Nix workflow.
