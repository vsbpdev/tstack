---
name: modernize
description: >-
  Assess and update an aging pull request. Use when asked to modernize or
  rebase a PR.
---

# Modernize

Modernize the idea, not just the branch. A clean rebase does not prove that the
PR still belongs in the product.

## Guardrails

- Fetch the latest tips of the actual PR head and base before starting.
- Treat existing worktree changes as user-owned. Use a disposable worktree when
  the active worktree is dirty or is being used for other work.
- Preserve published history. Rebase a local copy to assess compatibility.
  To update the existing PR, merge its base into the PR branch when the
  repository permits it and the update is authorized. When policy requires a
  linear history, prepare a rebased local branch and report it. Publish a
  replacement branch or PR only when authorized.
- Do not post a PR comment unless the user asked for one. Use the
  `leaving-pr-comments` skill for every comment posted on the user's behalf.

## Establish the PR's intent

Resolve the PR and record its URL, state, base, head repository and branch,
current head SHA, author, checks, and review state. Read the title, body, linked
issues, commits, review threads, and complete diff. State the smallest coherent
goal the PR was trying to accomplish before judging its implementation.

Fetch the latest remote base and PR head. Use the PR's actual base branch,
usually `main`, rather than assuming the local branch is current. Record the
merge base so the original PR and subsequent base-branch changes can be
compared separately.

## Check what changed around it

Inspect commits and diffs added to the base branch since the PR diverged. Search
merged and open PRs when titles, code paths, or behavior suggest a replacement
or competing implementation.

Look beyond overlapping lines:

- Has the same user-facing problem already been solved?
- Did APIs, types, schemas, dependencies, or architectural boundaries change?
- Do both sides now claim ownership of the same behavior or invariant?
- Was the code being changed deleted, moved, or intentionally simplified?
- Would landing the PR restore an old behavior or create two ways to do the
  same thing?
- Is the original benefit still large enough to justify the maintenance cost?

Do not describe a PR as current merely because Git reports no conflicts.

## Choose a disposition before pushing

Use one of these recommendations:

- **MODERNIZE**: the goal is still useful and the implementation remains the
  right shape. Update the branch using a method allowed by the guardrails,
  resolve conflicts, and test.
- **REWORK**: the goal is still useful, but a mechanical rebase would preserve
  the wrong implementation. Explain the smallest credible rewrite and stop
  before pushing unless the user also asked for that rework.
- **CLOSE**: the PR is superseded, obsolete, directionally wrong, or no longer
  worth its cost. Cite concrete evidence and stop. Do not spend time rebasing an
  obsolete branch, and do not close it without explicit permission.

Be willing to recommend `CLOSE`. Age alone is not evidence, and sunk effort is
not a reason to keep a PR alive.

## Update the branch

For a `MODERNIZE` disposition:

1. Create a safe local branch or disposable worktree at the recorded PR head.
2. Update it from the freshly fetched base using the allowed method.
3. Resolve conflicts using the PR's intent and the base branch's current
   behavior. Reconsider the disposition if the conflict reveals a product or
   architecture disagreement.
4. Review the final diff against the base. For a local rebase, also compare
   the commit series with `git range-diff`. Check for lost behavior and
   reintroduced changes.
5. Run focused checks for the affected area. Distinguish failures caused by
   the change from pre-existing or infrastructure failures.
6. Before an authorized push, confirm the remote head still matches the one
   you reviewed. If it moved, fetch and reassess.

Do not merge the PR.

## Report

Lead with `MODERNIZE`, `REWORK`, or `CLOSE` and the reason. Then summarize:

- meaningful changes on the base branch;
- conceptual conflicts or confirmation that none were found;
- the branch update and push result, if performed;
- checks run and their results;
- the remaining decision or work, with a link to the PR.

Separate observed facts from judgment. Say what could not be verified instead
of manufacturing confidence.
