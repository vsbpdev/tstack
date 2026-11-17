---
name: file-pr
description: File a concise pull request. Use when the user asks to file, open, or create a PR.
---

# File PR

Confirm the repository is approved for publication and the user authorized
a PR there. Verify the target owner, repository, and base. Check for an existing
PR on this branch and review the diff against its intended base before uploading.
Confirm the remote head contains the intended commits and excludes unrelated
work. Push the relevant branch only when authorized.

PR titles usually become commit messages, so follow the repository's title
conventions. Look at recently merged PRs and Git history for examples. Prefer a
concise, human-readable title that explains why the change matters:

BAD

> ❌ perf(server): negotiate permessage-deflate on the websocket

GOOD

> ✅ perf(server): cut websocket frame size by 70%+ with gzipping

Open the description with a simple explanation of the problem based on the
user's original prompt, then briefly explain the solution. Do not lead with an
implementation inventory:

BAD

> ❌ Removed implicit workspace carry-over from every "new thread" entry point (cmd+n / cmd+shift+o, sidebar v1/v2 buttons, command palette). New threads inherit only the project from context; branch, worktree, and env mode always come from the configured defaults. Deleted buildContextualThreadOptions, startNewThreadInProjectFromContext, and the v1 sidebar's seed-context machinery.

GOOD

> ✅ My "new worktree" default was ignored when starting new threads on existing worktrees. Super unintuitive. Now your preferences always apply.

Include verification only for checks you actually ran. Keep public titles,
descriptions, and commit messages focused on the problem, change, and
evidence. Omit model names, agent and harness credits, session links, internal
review reports, and generated co-author trailers. Verify human credits when
due. Preview the final text and attachments for private information and secrets.

Use the requested draft status or the repository's convention. If
`gh pr create` hits the GraphQL rate limit, check whether the PR was created
before retrying through REST. Preserve the intended target, head, base, and draft
status, and submit the reviewed body from a file. If the user also asked to
babysit it, continue with the [babysit-pr](../babysit-pr/SKILL.md) skill.

Do not post a follow-up PR comment unless it serves the user's request. When a
comment is needed, use the `leaving-pr-comments` skill.
