+++
title = "This Week in Ekala #8"
date = 2026-11-13
description = "EkaOS VM tests that really boot, an in-process switch engine, Nix 2.3 compatibility, mass package additions, and eka-ci channels"

[taxonomies]
tags = ["progress", "corepkgs", "ekapkgs", "ekapkgs-cli", "eka-ci", "cuda-pkgs", "ekapkgs-update", "python-packages"]
+++

Welcome to This Week in Ekala #8! Here's what happened this week.

<!-- more -->

## Highlights

- The EkaOS VM test suite now really boots and runs its assertions. This exposed real bugs that are now fixed: the systemd-boot installer, declarative unit enablement, service PATH handling, and 580 missing modules in ekapkgs.
- ekapkgs-cli gained `ekaos-switch`, an in-process switch engine. `ekapkgs system` now uses it to activate EkaOS generations, and a VM test covers it end to end.
- corepkgs, ekapkgs and python-packages can now be imported with Nix 2.3, an early milestone from the roadmap.
- A large wave of packages landed: roughly 200 new ekapkgs packages, about 637 Python package updates, and 13 database packages plus 7 PostgreSQL extensions from [@ic4-y](https://github.com/ic4-y).

## corepkgs

About 100 packages were updated in the monthly November update, the Linux kernel was bumped, `nurl` was added, and 16 smaller fixes landed. The fixes cover evaluation errors in variant package sets, compiler workarounds, and ekaos test flakiness.

- [gcc16: init](https://github.com/ekala-project/corepkgs/pull/255)<br>
  GCC 16 is now available, so packages can start being tested against the next compiler release.
- [pins: fall back to fetchTarball/fetchGit on Nix 2.3](https://github.com/ekala-project/corepkgs/pull/256)<br>
  Adds a small `fetch-locked.nix` helper, shared with ekapkgs and python-packages, so pins resolve on Nix 2.3, which lacks `builtins.fetchTree`. This is the first step toward the roadmap goal of Nix 2.3 compatibility.
- [treewide: avoid path interpolation for Nix 2.3](https://github.com/ekala-project/corepkgs/pull/257)<br>
  Removes path-literal interpolation, which Nix 2.3 cannot parse, so `ci/eval.nix` evaluates on older Nix.
- [ekaos/facter: use lib.groupBy instead of builtins.groupBy](https://github.com/ekala-project/corepkgs/pull/258)<br>
  Swaps a newer builtin for its `lib` equivalent, which completes Nix 2.3 evaluation of corepkgs, including EkaOS facter support.
- [ci: evaluate variant package sets](https://github.com/ekala-project/corepkgs/pull/265)<br>
  CI now evaluates every package inside each variant's own package set (`lua.v5_1.pkgs`, `python3.v3_12.pkgs`, `perl.v5_40.pkgs`, and so on). This surfaced a batch of hidden evaluation failures, which follow-up PRs fixed this week.
- [Darwin fixes](https://github.com/ekala-project/corepkgs/pull/252)<br>
  A round of aarch64-darwin fixes moved Darwin to LLVM 22 and repaired builds for gawk, erlang, ICU, gcc13, cyrus_sasl and others, keeping Apple Silicon support moving forward.
- [home: fail the combined activation when a user's activation fails](https://github.com/ekala-project/corepkgs/pull/264)<br>
  The combined home activation now exits non-zero when any user's activation fails. Previously it always exited 0, so `ekapkgs home switch` never triggered its automatic rollback.
- [home: only replace files that still link into the store](https://github.com/ekala-project/corepkgs/pull/263)<br>
  Fixes every home activation after the first one failing on read-only copied files. Activation also no longer overwrites files the user has replaced with their own.
- [home: enable user units declaratively through .wants links](https://github.com/ekala-project/corepkgs/pull/266)<br>
  User services declared by home modules (such as `mpd`, `dunst` and `postgresql`) now start at login without a manual `systemctl --user enable`.
- [ekaos/test-driver: implement execute() via a virtio-console backdoor](https://github.com/ekala-project/corepkgs/pull/267)<br>
  `Machine.execute()` was a stub that always succeeded, so every EkaOS VM test assertion passed without checking anything. A real backdoor shell over virtio-console means the tests now actually test something.
- [python2: remove the Python 2 package sets](https://github.com/ekala-project/corepkgs/pull/276)<br>
  `python2Packages` and `pypy2Packages` now throw a removal message, because they had diverged too far from PyPI to maintain. The `python2` and `pypy2` interpreters themselves remain.
- [ekaos/tests: make the VM test suite boot and pass](https://github.com/ekala-project/corepkgs/pull/282)<br>
  The EkaOS VM tests had never actually booted. The harness now boots through systemd-boot with an initrd-mounted root, and the stale tests have been updated so the whole suite passes.
- [ekaos: fix the service runtime gaps found by the VM tests](https://github.com/ekala-project/corepkgs/pull/281)<br>
  Fixes several runtime problems the newly working VM tests uncovered, including giving every generated service a sensible default PATH (EkaOS has no `/usr`) and applying `environment.etc` file modes.
- [ekaos/systemd-boot: fix the boot loader installer](https://github.com/ekala-project/corepkgs/pull/279)<br>
  The systemd-boot installer had never worked on an EkaOS VM; it does now. `system.build.toplevel` also always includes the initrd, making every generation bootable.
- [ekaos: switch-engine contract (unit keys, switch hints, /etc pruning)](https://github.com/ekala-project/corepkgs/pull/277)<br>
  EkaOS generations now emit the unit keys and metadata that ekapkgs-cli's new in-process switch engine reads, and match nixpkgs' switch-to-configuration-ng so the classification logic can be shared.
- [ekaos: enable units declaratively and reconcile unit directories](https://github.com/ekala-project/corepkgs/pull/262)<br>
  Services such as `crond` and `openssh` were never started at boot, because `WantedBy=` had no effect without `systemctl enable`. Units are now enabled declaratively through generated `.wants` links, so `services.*` modules work as expected.

## ekapkgs

Core was bumped, `open-code-review` was updated, pins gained the Nix 2.3 fallback, and several smaller fixes and cleanups landed.

- [Database packages and PostgreSQL extensions](https://github.com/ekala-project/ekapkgs/pulls?q=is%3Apr+author%3Aic4-y+is%3Amerged)<br>
  Added 13 database packages (including qdrant, milvus, mongodb-ce, rethinkdb, faiss and opentsdb) and 7 PostgreSQL extensions (including timescaledb and pg_hint_plan), making ekapkgs much more useful for data infrastructure. Contributed by [@ic4-y](https://github.com/ic4-y).
- [More packages!](https://github.com/ekala-project/ekapkgs/pull/138)<br>
  An 850+ commit push that added roughly 200 packages (many Perl modules, desktop applications, fonts and Ruby gems) along with a batch of build fixes, further closing the gap with nixpkgs.
- [ekaos: fix module list pointing at deleted modules, add CI check](https://github.com/ekala-project/ekapkgs/pull/144)<br>
  580 of the 708 entries in the EkaOS module list pointed at deleted files, so every EkaOS system build failed immediately. A new CI check keeps the list in sync.
- [flake: re-export ekaosSystem](https://github.com/ekala-project/ekapkgs/pull/145)<br>
  Brings back the documented `ekapkgs.ekaosSystem` flake function, so users can build EkaOS systems directly from the ekapkgs flake again.
- [kdePackages: fix evaluation errors, evaluate kdePackages in CI](https://github.com/ekala-project/ekapkgs/pull/147)<br>
  Fixed about 50 of the 487 KDE derivations that failed to evaluate. CI now descends into the `kdePackages` scope, so regressions like these get caught.

## ekapkgs-cli

`ekapkgs-serve` starts up faster and gained independent options to compress chunks and NARs, and `nix build --json` output for store-path installables is now handled correctly.

- [serve: simplify to single zstd-3 chunk compression level](https://github.com/ekala-project/ekapkgs-cli/pull/32)<br>
  Replaced the two-tier compression scheme and background recompression with a single inline zstd-3 pass, removing a lot of complexity at a good speed/ratio tradeoff.
- [serve: reduce memory pressure from GC, NAR streaming, and compression](https://github.com/ekala-project/ekapkgs-cli/pull/33)<br>
  O(1) GC reference checks and chunk-by-chunk NAR streaming keep `ekapkgs-serve` memory bounded, even when serving large store paths.
- [serve: add hourly cache history snapshots with CSV export](https://github.com/ekala-project/ekapkgs-cli/pull/34)<br>
  Cache metrics are now recorded every hour and can be exported with `ekapkgs cache history`, giving operators a view of cache behaviour over time.
- [Fix castore pull end to end: NixOS module, runtime closure, import names, signatures](https://github.com/ekala-project/ekapkgs-cli/pull/37)<br>
  Fixes found while deploying `ekapkgs-serve` with the castore backend on a real NixOS machine. Chunk-negotiated pulls now work end to end.
- [Prefetch only what a build actually needs](https://github.com/ekala-project/ekapkgs-cli/pull/38)<br>
  Commands such as `build`, `run` and `develop` no longer prefetch the entire build-time closure when outputs are already cached. For example, `dry-run nixpkgs#hello` no longer reports hundreds of paths to build.
- [home: reconcile systemd user units on switch and rollback](https://github.com/ekala-project/ekapkgs-cli/pull/40)<br>
  `ekapkgs home switch` now reloads the user manager and starts, stops or restarts changed user services, so home-declared services take effect immediately.
- [home: activate only the invoking user's home on switch](https://github.com/ekala-project/ekapkgs-cli/pull/41)<br>
  `home switch` no longer runs every configured user's activation as the invoking user, which could write other users' files into your home directory.
- [ekaos-switch: init the in-process switch engine](https://github.com/ekala-project/ekapkgs-cli/pull/42)<br>
  Adds `ekaos-switch`, an in-process Rust port of nixpkgs' switch-to-configuration-ng for EkaOS generations. This is the foundation for replacing the `switch-to-configuration` subprocess.
- [system: switch ekaos toplevels in-process with ekaos-switch](https://github.com/ekala-project/ekapkgs-cli/pull/43)<br>
  `ekapkgs system switch`, `boot`, `test`, `rollback` and `update` now share one activation path. EkaOS generations are activated in-process by the new engine.
- [nix: VM test for ekapkgs system switching ekaos](https://github.com/ekala-project/ekapkgs-cli/pull/44)<br>
  A new VM test boots an EkaOS generation and drives `ekapkgs system` inside the guest, covering service restarts and reloads, `/etc` pruning, `test`, `boot`, `switch` and `rollback` end to end. It also caught several bugs in the engine before release.
- [home: restore the active generation when switch fails](https://github.com/ekala-project/ekapkgs-cli/pull/39)<br>
  A failed `home switch` now rolls back to the generation that was actually active. Previously it rolled back one generation too far.
- [Evaluate with iets when it is installed](https://github.com/ekala-project/ekapkgs-cli/pull/46)<br>
  When [iets](https://github.com/numtide/iets), an alternative Nix evaluator, is installed, ekapkgs-cli uses it for evaluation and falls back to `nix` for everything else, matching eka-ci's move to iets.

## eka-ci

The NixOS module and overlay were cleaned up, channel configuration moved from a list to an attribute set, and the server now logs a startup health summary.

- [Index creation](https://github.com/ekala-project/eka-ci/pull/218)<br>
  eka-ci now generates package indexes whenever a channel updates.
- [Channel promotion](https://github.com/ekala-project/eka-ci/pull/219)<br>
  Channels can now be triggered by GitHub push events, so promotion happens automatically when the target branch moves.
- [Channel config](https://github.com/ekala-project/eka-ci/pull/221)<br>
  Defines how channels are described and moves evaluation from nix-eval-jobs to iets' `eval-jobs`, with provenance-based URI restrictions replacing `allowed_uris`.

## cuda-pkgs

Unused files were removed, the order in which overlays apply was fixed, and `default.nix` now accepts `config` so unfree CUDA packages evaluate.

- [cudaPackages: apply config.overlays.cuda to every scope](https://github.com/ekala-project/cuda-pkgs/pull/2)<br>
  Adds a module-level `overlays.cuda` option that applies to every CUDA version's scope. Users can now add or override CUDA packages across all versions, as the roadmap asks of every ecosystem set.

## ekapkgs-update

Updates now keep passthru metadata and use an update version script, and locating a package's file falls back to `meta.position`.

- [Watch daemon](https://github.com/ekala-project/ekapkgs-update/pull/60)<br>
  A new `watch` command subscribes to upstream releases and automatically opens update PRs against the corresponding repository.
- [Populate and fully implement the web dashboard](https://github.com/ekala-project/ekapkgs-update/pull/62)<br>
  `run` and `watch` now record per-phase progress, failures and CVE counts across repositories, and the dashboard is fully built out to show it. `watch` also gained an observe-only bootstrap, so it dispatches only new releases.

## python-packages

Pins gained the Nix 2.3 fallback, and flake `legacyPackages` evaluation was fixed.

- [python3Packages: November updates and build fixes](https://github.com/ekala-project/python-packages/pull/21)<br>
  About 637 Python packages were updated by an automated `ekapkgs-update` run, followed by a pass that fixed every resulting build failure.
