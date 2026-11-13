+++
title = "This Week in Ekala #7"
date = 2026-11-01
description = "GCC 15 and Python defaults, pkg-config and CMake hardening, PostgreSQL extensions, mass package additions, and async ekapkgs-cli"

[taxonomies]
tags = ["progress", "corepkgs", "ekapkgs", "ekapkgs-cli"]
+++

Welcome to This Week in Ekala #7! Here's what happened this week.

<!-- more -->

## Highlights

- corepkgs now defaults to GCC 15 and a newer Python.
- New checks confirm that `.pc` and `.cmake` files across corepkgs are valid, and the packages that failed them were fixed.
- ekapkgs gained 48 PostgreSQL extensions plus hundreds of new packages across several mass-addition PRs.
- ekapkgs-cli moved fully to async I/O, added a build progress bar, and can now recover from a corrupted database.

## corepkgs

All four corepkgs PRs this week were significant toolchain and quality changes.

- [Bump python default](https://github.com/ekala-project/corepkgs/pull/243)<br>
  Moved the default Python interpreter to a newer CPython release, so downstream packages build against a current Python by default.
- [treewide: apply best-practices, add missing meta.platforms](https://github.com/ekala-project/corepkgs/pull/245)<br>
  Added missing `meta.platforms` to about 220 packages and removed outdated patterns across 271 files, so platform support is declared explicitly and the tree follows one consistent style.
- [Pkg-config and cmake testing and hardening](https://github.com/ekala-project/corepkgs/pull/246)<br>
  Introduced tests that check whether installed `.pc` and `.cmake` files are valid, and fixed the issues found in over a hundred packages. Broken build metadata is now caught before it reaches downstream consumers.
- [GCC 15](https://github.com/ekala-project/corepkgs/pull/247)<br>
  Switched the default compiler to GCC 15, which keeps Ekala on the latest toolchain. Packages that broke under the new compiler were fixed in the same PR.

## ekapkgs

Hundreds of packages were added, and corepkgs was bumped to pick up the latest toolchain changes.

- [Mass fixes](https://github.com/ekala-project/ekapkgs/pull/133)<br>
  Fixed broken builds in 143 files, improving the overall health of the package set.
- [More mass additions](https://github.com/ekala-project/ekapkgs/pull/134)<br>
  A large batch of new packages, touching 234 files, broadens the coverage of ekapkgs.
- [Mass additions 3](https://github.com/ekala-project/ekapkgs/pull/135)<br>
  Added another 254 files of packages, including much of the KDE PIM stack (Akonadi, Akregator, and friends), building on last week's KDE packaging work.
- [postgresql: 48 extensions added](https://github.com/ekala-project/ekapkgs/pulls?q=is%3Apr+is%3Amerged+author%3Aic4-y+postgresql)<br>
  Added a wide set of PostgreSQL extensions, including citus, pgvector, pg_cron, Apache AGE, h3-pg, pgaudit, pg_partman, and pg_repack, making ekapkgs a solid base for PostgreSQL deployments. Contributed by [@ic4-y](https://github.com/ic4-y).
- [More additions](https://github.com/ekala-project/ekapkgs/pull/136)<br>
  The largest batch of the week, touching 365 files, including the 0 A.D. game, AppArmor tooling, and accessibility tools.

## ekapkgs-cli

Earlier async work landed in [#30](https://github.com/ekala-project/ekapkgs-cli/pull/30) and was finished by the PR below.

- [Async, UI, and corrupt recovery](https://github.com/ekala-project/ekapkgs-cli/pull/31)<br>
  Removed the last blocking I/O calls in favor of tokio, added a progress bar to Nix builds, and made the CLI recover gracefully from a corrupted database. The result is more responsive and more resilient.
