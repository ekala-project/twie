+++
title = "This Week in Ekala #2"
date = 2026-09-24
description = "PulseAudio and GStreamer stacks, Meson and Go/Rust builder improvements, dev shells, and major ekapkgs-cli advances"

[taxonomies]
tags = ["progress", "corepkgs", "ekapkgs-cli", "eka-ci"]
+++

Welcome to This Week in Ekala #2! Here's what happened this week.

<!-- more -->

## Highlights

- Full PulseAudio and GStreamer multimedia stacks now build in corepkgs, unblocking ISO images and desktop audio
- Meson and Go/Rust builders gained structured attribute support, reducing boilerplate and shrinking dependency chains
- ekapkgs-cli added package templating, fuzzy search, tab completions, and hardened CAS storage
- eka-ci reached end-to-end functionality with passthru test discovery

## corepkgs

4 packages were added as build-fix dependencies and several existing packages received build corrections.

- [Pulseaudio](https://github.com/ekala-project/corepkgs/pull/199)<br>
  Ported PulseAudio and its dependency chain — flac, libsndfile, mpg123, speex, speexdsp, and webrtc-audio-processing — enabling audio support for QEMU images and minimal systems.
- [Gstreamer](https://github.com/ekala-project/corepkgs/pull/202)<br>
  Added the full GStreamer multimedia stack: core, base, good, bad, ugly, libav, devtools, ges, and rtsp-server, along with supporting libraries like graphene, orc, and taglib.
- [Pulseaudio: fully build](https://github.com/ekala-project/corepkgs/pull/204)<br>
  Completed PulseAudio by porting all optional dependencies — libjack2, lirc, sbc, waf, and others — so it builds fully for ISO images and minimal systems.
- [Meson enhancements](https://github.com/ekala-project/corepkgs/pull/206)<br>
  Introduced `mesonFeatures` for handling enabled/disabled/auto flags and improved `mesonEntries` to infer Nix booleans as Meson booleans, significantly reducing boilerplate across dozens of Meson-built packages.
- [Prune testing from Go and Rust builds](https://github.com/ekala-project/corepkgs/pull/207)<br>
  Eliminated redundant test builds from the Go and Rust builders by automatically adding version checks to `passthru.tests`, shortening build times and shrinking dependency chains.
- [GTK / Hardware improvements](https://github.com/ekala-project/corepkgs/pull/214)<br>
  Added GDB, valgrind, and split linux-firmware packages (per-vendor WiFi, Bluetooth, and GPU firmware), plus a firmware-aware hardware facter module for automatic firmware selection.
- [Finish Dev shells](https://github.com/ekala-project/corepkgs/pull/216)<br>
  Added modular dev shell support with dotenv, enterShell, env, files, packages, scripts, and treefmt modules for language ecosystems. Linters and LSPs were moved to ekapkgs.

## ekapkgs-cli

- [Harden CAS storage, server security, and client cache robustness](https://github.com/ekala-project/ekapkgs-cli/pull/6)<br>
  Migrated CAS directory storage to SQLite with transactional ingest, added blake3 digest verification, GC primitives, path traversal prevention, and improved client-side chunk cache eviction.
- [Add package management improvements and tab completions](https://github.com/ekala-project/ekapkgs-cli/pull/7)<br>
  Added a store path index, symlink directory management, package name validation with fuzzy suggestions, background home apply via systemd timers, and dynamic tab completion for packages and services.
- [Improve package search with fuzzy matching and remote indexes](https://github.com/ekala-project/ekapkgs-cli/pull/8)<br>
  Package search now supports fuzzy matching with highlighted results and auto-downloads pre-built search indexes from configurable remote URLs.
- [Update nix packaging, systemd hardening, and documentation](https://github.com/ekala-project/ekapkgs-cli/pull/9)<br>
  Added CAS storage backend documentation, systemd hardening for the server module, and new docker, drv-diff, and wtf subcommands.
- [Add templating](https://github.com/ekala-project/ekapkgs-cli/pull/11)<br>
  Integrated nix-template functionality for scaffolding new package expressions, with automatic build system detection and URL-based source prefetching.

## eka-ci

- [Get e2e working](https://github.com/ekala-project/eka-ci/pull/208)<br>
  Brought the CI pipeline to end-to-end functionality with passthru test discovery, jobset filtering, and improved Nix evaluation handling.
