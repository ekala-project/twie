---
name: add-entry
description: Add a PR entry to the latest This Week in Ekala blog post
arguments: [url]
---

# Add Blog Entry

Add a new entry to the latest "This Week in Ekala" blog post from the given PR: $url

## Steps

1. **Fetch the PR** — Use WebFetch on `$url` to extract the PR title, author, description, and repository name (the `org/repo` from the URL path).

2. **Find the latest blog post** — Glob for `content/blog/????-??-??-this-week-in-ekala-*.md` and pick the most recent file (highest number suffix). Read that file.

3. **Determine the repository section** — Extract the repo name from the PR URL (e.g. `corepkgs` from `ekala-project/corepkgs`). Check if a `## {repo-name}` section already exists in the post.

4. **Add the entry:**
   - If the section exists, append the entry to the end of that section (before the next `##` heading or end of file).
   - If the section does NOT exist, add a new `## {repo-name}` section at the end of the file, then add the entry under it.

5. **Verify** — Run `nix-shell -p zola --run "zola build"` to confirm the site still builds.

6. **Commit** — Stage the modified blog post file and commit with the message `YYYY-MM-DD: <PR title>`, where `YYYY-MM-DD` is today's date and `<PR title>` is the title of the PR that was added.

## Entry Format

Each entry is exactly two lines within a markdown list:
- First line: `- [PR title](PR url)<br>` — note the `<br>` at the end for a visible line break
- Second line (indented with 2 spaces): a brief description of what the PR does, with contributor attribution if the author is not `jonringer`.

Example:
```
- [treewide: drop x86_64-darwin remnants](https://github.com/ekala-project/corepkgs/pull/182)<br>
  Removed legacy x86_64-darwin configuration remnants across the codebase. Contributed by [@qweered](https://github.com/qweered).
```

## Important

- Do NOT create a new blog post file. Only add to the existing latest post.
- Do NOT add duplicate entries. If the PR URL already exists in the file, inform the user and stop.
- Keep the description to one or two sentences.
- The `[taxonomies] tags` in the frontmatter should include the repo name if not already present.
