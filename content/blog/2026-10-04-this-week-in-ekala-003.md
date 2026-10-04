+++
title = "This Week in Ekala #3"
date = 2026-10-04
description = "FUSE library serving, gRPC builders, MCP diagnostics, CA store bloom filters, and dev shell tooling"

[taxonomies]
tags = ["progress", "corepkgs", "ekapkgs", "ekapkgs-cli", "eka-ci"]
+++

Welcome to This Week in Ekala #3! Here's what happened this week.

<!-- more -->

## Highlights

- ekapkgs-cli gained a FUSE subcommand that resolves shared library sonames to Nix packages on demand, enabling transparent library loading for unpatched binaries via nix-ld
- eka-ci replaced SSH-based build coordination with gRPC and added an MCP server endpoint so AI agents can diagnose failing PR gates
- CA store now uses bloom filters for chunk negotiation, cutting redundant transfers
- Dev shell modules for 15 language ecosystems landed in ekapkgs

## corepkgs

- [Clean up pkgs](https://github.com/ekala-project/corepkgs/pull/215)<br>
  Removed ~180 lines of stale internal aliases and migrated them to the dedicated nixpkgs compatibility alias file, simplifying the top-level package set.
- [config: enable allowUnfree by default, blocklist non-redistributable licenses in CI](https://github.com/ekala-project/corepkgs/pull/217)<br>
  Changed `allowUnfree` to default to true so firmware and redistributable driver packages evaluate without extra configuration, while blocking non-redistributable licenses from the CI binary cache.

## ekapkgs

- [mkDevShell refactor](https://github.com/ekala-project/ekapkgs/pull/17)<br>
  Added 15 language-specific dev shell tool modules (Rust, Go, Python, Java, Ruby, Zig, and more) and ported packages like dart, elm, helm, mold, and ibus from corepkgs.
- [Writers and eval fixes](https://github.com/ekala-project/ekapkgs/pull/18)<br>
  Brought in dev shell modules, new packages (ansible, bitwarden-cli, arc-theme, bamf), and eval/CI alignment fixes following the corepkgs alias cleanup.

## ekapkgs-cli

- [Finish CA Store](https://github.com/ekala-project/ekapkgs-cli/pull/12)<br>
  Implemented bloom filter–based chunk negotiation to skip already-stored chunks, added NAR decomposition for granular deduplication, and expanded garbage collection and integration tests.
- [Add fuse subcommand for on-demand /usr/lib shared library serving](https://github.com/ekala-project/ekapkgs-cli/pull/13)<br>
  Mounts a read-only FUSE filesystem that resolves sonames to Nix packages via a CI-generated index, downloading them on demand through the existing negotiate protocol for seamless nix-ld integration.

## eka-ci

- [GRPC builder support](https://github.com/ekala-project/eka-ci/pull/209)<br>
  Replaced SSH-based build coordination with a gRPC protocol, added a circuit breaker for builder health management, and introduced a new `nix_store` crate for direct Nix daemon communication.
- [Search Index DB generation](https://github.com/ekala-project/eka-ci/pull/210)<br>
  Generates SQLite search indexes for packages and files when channels are pushed, enabling the ekapkgs-cli search and FUSE features to use pre-built indexes from the CI server.
- [Add MCP server endpoint for AI agent PR diagnostics](https://github.com/ekala-project/eka-ci/pull/211)<br>
  Embedded a Model Context Protocol server at `/v1/mcp` that exposes tools for listing failing PRs, reading build logs, and tracing dependency failure chains, letting AI agents diagnose CI issues.
- [Maintenance: git checkout of new SHAs, config alias, docs, Nix packaging](https://github.com/ekala-project/eka-ci/pull/212)<br>
  Fixed git worktree checkout for new commits, added config key aliases, hardened the NixOS module service PATH, and resolved Nix build issues. Contributed by [@znaniye](https://github.com/znaniye).
