+++
title = "This Week in Ekala #5"
date = 2026-10-18
description = "PostgreSQL extension builders, stdenv hook refactoring, CMake modernization, home modules, and CLI performance"

[taxonomies]
tags = ["progress", "corepkgs", "ekapkgs", "ekapkgs-cli", "ekapkgs-update"]
+++

Welcome to This Week in Ekala #5! Here's what happened this week.

<!-- more -->

## Highlights

- PostgreSQL gained `pg_config`, a PGXS extension builder, and a dedicated `postgresPackages` namespace
- Build phases no longer need to manually call pre/post hooks — `setup.sh` handles them as first-class actions
- ekapkgs added 20+ home-manager modules and popular desktop packages like Chromium, Electron, and VS Code
- The ekapkgs CLI got chunk compression, cache metrics, and new service/dev-shell scaffolding commands

## corepkgs

3 best-practice sweep PRs and a downstream-fixes PR landed, normalizing package definitions across the tree.

- [postgresql: expose pg_config and add PGXS extension builder](https://github.com/ekala-project/corepkgs/pull/230)<br>
  Ported relocatable `pg_config` and a PGXS extension builder from nixpkgs, unblocking `buildPgrxExtension` and all PGXS-based PostgreSQL extensions. Contributed by [@ic4-y](https://github.com/ic4-y).
- [postgresPackages: init](https://github.com/ekala-project/corepkgs/pull/236)<br>
  Created a dedicated `postgresPackages` namespace and overlay for PostgreSQL extensions.
- [stdenv: support extensions attr](https://github.com/ekala-project/corepkgs/pull/232)<br>
  Added an `extensions` attribute to `mkDerivation`, allowing optional compile-time variants (e.g. `gtk`, `odbc`) to be communicated without clobbering the package name.
- [Promote pre+post hooks to separately defined actions](https://github.com/ekala-project/corepkgs/pull/231)<br>
  Made pre/post hooks first-class citizens in `setup.sh` so build phases no longer need to invoke them manually, simplifying every builder in the tree.
- [Java v8 and utilities / python: use mkManyVariants](https://github.com/ekala-project/corepkgs/pull/233)<br>
  Added Java v8, `fetchMavenArtifact`, and JRE certificate generation, and [migrated Python](https://github.com/ekala-project/corepkgs/pull/220) to `mkManyVariants` enabling `python.extendVariants`.
- [CMake entries rewrite](https://github.com/ekala-project/corepkgs/pull/238)<br>
  Continued the transition from `cmakeFlags` to the structured `cmakeEntries` API across LLVM, protobuf, and dozens of other packages.

## ekapkgs

23 database and data-infrastructure packages were added (Kafka, Cassandra, Neo4j, SurrealDB, and more), 3 C/C++/Zig packages landed, and a best-practices sweep fixed up existing package definitions. Contributed by [@ic4-y](https://github.com/ic4-y).

- [More home modules](https://github.com/ekala-project/ekapkgs/pull/79)<br>
  Added 20+ home-manager modules including programs (emacs, fish, zsh, tmux, kitty, ghostty, mpv, zathura) and services (dunst, mako, mpd, gammastep, cliphist, udiskie, darkman).
- [Add popular packages](https://github.com/ekala-project/ekapkgs/pull/80)<br>
  Brought in Chromium, Firefox build support (`buildMozillaMach`), Electron, VS Code, Bitwarden Desktop, Dolphin Emulator, Deluge, and many more popular desktop and GUI applications.

## ekapkgs-cli

2 incremental PRs improved home/system commands and added cache usage inspection.

- [Service improvements](https://github.com/ekala-project/ekapkgs-cli/pull/20)<br>
  Added `ekapkgs env`, `ekapkgs try`, and `ekapkgs status` commands along with service template scaffolding and agent skills for dev-shell and home-service creation.
- [Cache improvements](https://github.com/ekala-project/ekapkgs-cli/pull/19)<br>
  Introduced zstd compression for larger chunks and inlined sub-1KB chunks into B-tree pages, reducing storage overhead and improving cache read performance.

## ekapkgs-update

- [Add review command](https://github.com/ekala-project/ekapkgs-update/pull/57)<br>
  Added an `ekapkgs-update review` command for reviewing PRs, defaulting to `ekala-project/ekapkgs` but supporting any repository.
