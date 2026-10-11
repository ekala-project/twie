+++
title = "This Week in Ekala #4"
date = 2026-10-11
description = "A/B boot scheme, home environment modules, VM images, BEAM and Zig build support, and upstream polling"

[taxonomies]
tags = ["progress", "corepkgs", "ekapkgs", "ekapkgs-cli", "ekapkgs-update"]
+++

Welcome to This Week in Ekala #4! Here's what happened this week.

<!-- more -->

## Highlights

- EkaOS gains an A/B boot scheme with automatic fallback via systemd-boot boot counting
- New home/user environment module system with program modules for bash, git, gpg-agent, and ssh
- Full BEAM ecosystem support (Erlang, Mix, Rebar3) and Zig build helpers land in corepkgs
- `ekapkgs-update` can now poll RSS feeds for automated upstream release detection

## corepkgs

- [Home/user env](https://github.com/ekala-project/corepkgs/pull/221)<br>
  Introduced a home/user environment module system with program modules for bash, git, gpg-agent, and ssh, enabling end-to-end `ekapkgs home` workflows.
- [Pinning: allow for the repo pins to be committed to store](https://github.com/ekala-project/corepkgs/pull/222)<br>
  Repo pins are now rendered as store paths, preventing evaluation cache misses from unpopulated pins.
- [zig.fetchDeps, zig.buildZigPackage: init](https://github.com/ekala-project/corepkgs/pull/223)<br>
  Added a Zig fixed-output derivation fetcher and build helper, making it straightforward to package Zig projects.
- [ekaos/ab-boot: init A/B boot scheme](https://github.com/ekala-project/corepkgs/pull/186)<br>
  Implements A/B boot via systemd-boot boot counting on existing generation BLS entries. If the health check fails after N attempts, systemd-boot automatically falls back to the previous proven generation.
- [wrapDriverProgram: init](https://github.com/ekala-project/corepkgs/pull/99)<br>
  A new wrapper that creates a runtime glibc + driver library environment, allowing GUI and CUDA applications to run on non-Nix systems with hardware acceleration.
- [VM Image](https://github.com/ekala-project/corepkgs/pull/193)<br>
  EkaOS disk images now build and boot under QEMU, with a reworked initrd, improved systemd service management, and a boot-to-multiuser integration test.
- [beamPackages: init](https://github.com/ekala-project/corepkgs/pull/225)<br>
  Full BEAM ecosystem build support including Erlang, Mix, and Rebar3 builders, a Hex fetcher, and Mix/Rebar3 release helpers.

## ekapkgs

22 Go packages were added and Omarchy dependency packages were brought in.

- [Omarchy deps](https://github.com/ekala-project/ekapkgs/pull/22)<br>
  Added the dependency packages needed for Omarchy packaging, including libportal, webkitgtk, SDL2, and blender build fixes.
- [Add Go packages (22) from PR #21](https://github.com/ekala-project/ekapkgs/pull/23)<br>
  22 new Go packages built with `buildGoModule`, including hybrid Go+frontend packages with embedded pnpm/npm frontends.

## ekapkgs-cli

- [Home support](https://github.com/ekala-project/ekapkgs-cli/pull/15)<br>
  Implemented end-to-end `ekapkgs home` command for managing user environment configurations.
- [System feature parity](https://github.com/ekala-project/ekapkgs-cli/pull/16)<br>
  Brought `ekapkgs system` to near-parity with nixos-rebuild, supporting the core system management workflows.

## ekapkgs-update

- [Polling and minor fixes](https://github.com/ekala-project/ekapkgs-update/pull/55)<br>
  Added an RSS feed polling and watch subsystem for automated upstream release detection, along with database migrations and version preference fixes.
- [Version alignment](https://github.com/ekala-project/ekapkgs-update/pull/56)<br>
  Fixes stale commit-SHA revs when a version is bumped by adding a rev fixup rewriting pass during updates.
