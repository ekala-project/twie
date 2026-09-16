---
name: new-week
description: Create a new This Week in Ekala blog post for the current week
---

# New Week

Create a new "This Week in Ekala" blog post for the current week.

## Steps

1. **Determine the issue number** — Glob for `content/blog/????-??-??-this-week-in-ekala-*.md` and find the highest existing issue number. The new issue is that number + 1.

2. **Create the post** — Write a new file at `content/blog/{today's date}-this-week-in-ekala-{NNN}.md` where `NNN` is the zero-padded issue number (e.g. `002`, `013`).

3. **Use this template:**

```markdown
+++
title = "This Week in Ekala #{N}"
date = {today's date}
description = ""

[taxonomies]
tags = ["progress"]
+++

Welcome to This Week in Ekala #N! Here's what happened this week.

<!-- more -->
```

4. **Verify** — Run `nix-shell -p zola --run "zola build"` to confirm the site builds.

5. **Report** — Tell the user the file was created and the issue number. Remind them to fill in the `description` field in the frontmatter before publishing.

## Important

- Use today's date for both the filename and the `date` field.
- Do NOT add any repository sections or entries — those are added later with `/add-entry`.
- Do NOT commit. The user will commit when the post is ready.
