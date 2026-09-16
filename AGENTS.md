# This Week in Ekala — Agent Guide

## Blog Post Structure

Each weekly post is a Markdown file in `content/blog/` named `YYYY-MM-DD-this-week-in-ekala-NNN.md`.

Posts are organized into sections by repository, using `## repo-name` headings (e.g. `## corepkgs`).

## Entry Format

Each entry consists of exactly two lines:

1. A bullet with the PR title linked to the PR URL
2. An indented line describing the significance of the change

```markdown
- [PR title](https://github.com/ekala-project/repo/pull/NNN)<br>
  A statement about why this change matters or what it enables.
```

The `<br>` at the end of the first line ensures a visible line break between the linked title and the indented description.

Example:

```markdown
## corepkgs

- [treewide: drop x86_64-darwin remnants](https://github.com/ekala-project/corepkgs/pull/182)<br>
  Removed legacy x86_64-darwin configuration remnants, laying the groundwork for aarch64-darwin (Apple Silicon) support. Contributed by [@qweered](https://github.com/qweered).
- [Aliases: move to dedicated folder](https://github.com/ekala-project/corepkgs/pull/191)<br>
  Relocated `stdenv/aliases.nix` to a dedicated directory, separating internal Ekapkgs aliases from nixpkgs compatibility aliases for cleaner architecture and future independence.
```

## Rules

- Group entries under `## repo-name` sections based on the repository the PR belongs to.
- If a section for the repository does not exist yet, create one.
- Attribute contributors with `Contributed by [@user](profile-url).` unless the author is `jonringer`.
- Keep the description to one or two sentences focused on significance, not implementation detail.
- The site is built with Zola. Verify changes with `nix-shell -p zola --run "zola build"`.
