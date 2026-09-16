+++
title = "This Week in Ekala #2"
date = 2026-09-15
description = "Darwin cleanup, aarch64-darwin groundwork, and more"

[taxonomies]
tags = ["progress", "corepkgs", "darwin"]
+++

Welcome to This Week in Ekala! Here's what happened this week.

<!-- more -->

## corepkgs

- [treewide: drop x86_64-darwin remnants](https://github.com/ekala-project/corepkgs/pull/182)
  Removed legacy x86_64-darwin configuration remnants across the codebase, laying the groundwork for aarch64-darwin (Apple Silicon) support. Contributed by [@qweered](https://github.com/qweered).
- [Aliases: move to dedicated folder](https://github.com/ekala-project/corepkgs/pull/191)
  Relocated `stdenv/aliases.nix` to a dedicated directory, separating internal Ekapkgs aliases from nixpkgs compatibility aliases for cleaner architecture and future independence.
