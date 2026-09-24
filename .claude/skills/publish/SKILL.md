---
name: publish
description: Create a new This Week in Ekala blog post with auto-discovered PRs from all ekala-project repos
---

# Publish

Create a new "This Week in Ekala" blog post, auto-populated with merged PRs from ekala-project repositories.

## Repositories to scan

- `ekala-project/corepkgs`
- `ekala-project/ekapkgs`
- `ekala-project/ekapkgs-cli`
- `ekala-project/eka-ci`
- `ekala-project/haskell-pkgs`
- `ekala-project/cuda-pkgs`
- `ekala-project/ekapkgs-update`
- `ekala-project/python-packages`
- `ekala-project/ekafleet`

## Steps

### 1. Determine issue number and cutoff date

- Glob for `content/blog/????-??-??-this-week-in-ekala-*.md` and find the highest existing issue number. The new issue is that number + 1.
- Read the most recent post and extract the `date` field from its TOML frontmatter. This date is the cutoff — only PRs merged **after** this date will be included.

### 2. Discover merged PRs

For each repository listed above, run:

```
gh search prs --repo OWNER/REPO --merged --merged-at ">CUTOFF_DATE" --json number,title,repository --limit 100
```

Collect all results. Skip any repository that returns zero results.

### 3. Fetch PR details

For each discovered PR, run:

```
gh pr view NUMBER --repo OWNER/REPO --json title,author,body,url,mergedAt,files
```

Batch these calls where possible to avoid excessive serial round-trips.

### 4. Present PRs for user filtering

After fetching details, present all discovered PRs to the user in a table grouped by repo, showing PR number, title, merge date, and author. Propose a triage (significant vs. minor) for each PR based on:

- **Significant** — Major features, breaking changes, architectural changes, new subsystems, build system overhauls, multi-file refactors that change behavior.
- **Minor** — Single package additions (`init at`), version bumps, routine updates, small fixes, typo/docs-only changes.

**Ask the user explicitly** which PRs to include, which to omit, and whether they agree with the significant/minor classification. Wait for the user's response before proceeding. The user may:
- Remove entire repos from the post
- Remove individual PRs
- Reclassify PRs between significant and minor
- Add PRs that were missed

### 5. Write the blog post

Create a new file at `content/blog/{today's date}-this-week-in-ekala-{NNN}.md` where `NNN` is the zero-padded issue number.

Use today's date for both the filename and the frontmatter `date` field.

#### Template

```markdown
+++
title = "This Week in Ekala #{N}"
date = {today's date}
description = "{short thematic summary of the week's changes}"

[taxonomies]
tags = ["progress", "{repo1}", "{repo2}", ...]
+++

Welcome to This Week in Ekala #{N}! Here's what happened this week.

<!-- more -->

## Highlights

- {2-4 most notable changes as plain bullets}

## {repo-name}

{Summary sentence aggregating minor changes, e.g. "15 packages were added and 8 were updated."}

- [Significant PR title](PR url)<br>
  1-2 sentence description of significance.
```

#### Section rules

- Only create a `## repo-name` section for repos that had merged PRs in the period.
- Each repo section starts with a summary sentence that aggregates the minor PRs (package counts, update counts, fix counts).
- If a repo had **only** minor PRs, include just the summary sentence with no individual entries.
- Individual entries are only for **significant** PRs. Each entry follows the two-line format: `- [title](url)<br>` then an indented description.
- Order entries chronologically within each section (earliest merge first).
- Attribute contributors with `Contributed by [@user](https://github.com/user).` unless the author is `jonringer`.
- Keep descriptions to one or two sentences focused on significance, not implementation detail.
- The `[taxonomies] tags` should include `"progress"` plus the name of each repo that has a section.

#### Highlights section

Pick 2-4 of the most notable changes across all repos. Write them as plain bullet points (no links needed, but links are fine if helpful).

#### Frontmatter description

Write a short, comma-separated summary of themes (e.g. "Dev shell improvements, new package additions, and CI overhaul").

### 6. Verify

Run `nix-shell -p zola --run "zola build"` to confirm the site builds.

### 7. Report

Tell the user:
- The file that was created and the issue number
- The date range covered (cutoff date → today)
- How many PRs were found per repo and how many were significant vs. minor
- Remind them to review descriptions and highlights before committing

## Important

- Use today's date for both the filename and the `date` field.
- Do NOT commit. The user will commit when the post is ready.
- If no PRs are found across any repo, still create the post with the template and inform the user.
