+++
title = "This Week in Ekala #1"
date = 2026-09-15
description = "Darwin cleanup, alias reorganization, and the launch of This Week in Ekala"

[taxonomies]
tags = ["progress", "corepkgs", "darwin"]
+++

Welcome to the first issue of *This Week in Ekala*, a weekly summary of progress and community updates from the Ekala project.

<!-- more -->

## Highlights

- Launched the "This Week in Ekala" blog

## corepkgs

- [treewide: drop x86_64-darwin remnants](https://github.com/ekala-project/corepkgs/pull/182)<br>
  Removed legacy x86_64-darwin configuration remnants across the codebase, laying the groundwork for aarch64-darwin (Apple Silicon) support. Contributed by [@qweered](https://github.com/qweered).
- [Aliases: move to dedicated folder](https://github.com/ekala-project/corepkgs/pull/191)<br>
  Relocated `stdenv/aliases.nix` to a dedicated directory, separating internal Ekapkgs aliases from nixpkgs compatibility aliases for cleaner architecture and future independence.
- [Enable structured Attrs by default](https://github.com/ekala-project/corepkgs/pull/196)<br>
  Enabled `structuredAttrs = true` by default in stdenv, a deliberate breaking change from nixpkgs. Requirement for potential "nushell as builder" future migration.
- [Add config.overlays.linux](https://github.com/ekala-project/corepkgs/pull/198)<br>
  Added a dedicated option for Linux kernel overlays, allowing customization without needing to manipulate `kernelPackagesExtensions` directly.
- [Hardware enablement, ISO image](https://github.com/ekala-project/corepkgs/pull/189)<br>
  Added hardware enablement modules for audio, camera, fingerprint readers, printing, NVIDIA, power management, scanner, and Thunderbolt, along with an ISO image target.
- [CMake entries](https://github.com/ekala-project/corepkgs/pull/201)<br>
  Leverages `__structuredAttrs` to provide a more natural way to define CMake build configurations.
