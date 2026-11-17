---
name: babysit-pr
description: Monitor a pull request through review and CI. Use when the user asks to monitor, watch, or babysit a PR.
---

# Babysit PR

Confirm the PR, its current head, and what the user authorized. Monitoring
alone is read-only. Fix, push, post replies, resolve threads, or merge only
within the user's authorization and repository policy.

## Waiting

Never write your own watcher, polling loop, or `sleep` loop. Use one watcher
per PR: `watch_pull_request` in T3 Code, `pr-wait` everywhere else. Never run
both on the same PR. Every watcher on a GitHub account shares one rate limit.

### In T3 Code: `watch_pull_request`

Triage the existing checks and comments first. If work is still pending, call
`watch_pull_request` and end your turn. T3 Code wakes you with a message when
a check fails, the required checks pass, someone comments or reviews, or the
branch conflicts.

- **Only the thread that owns the PR watches it.** If you are a delegated
  child task (a reviewer, a Sol round, any `delegate_task` child), never call
  `watch_pull_request`. The parent owns the PR. Do not watch PRs you are not
  working on.
- **A wake is news, not a merge decision.** Check the merge gate yourself.
- **Unwatch when you hand off.** A watched thread stays in Working and out of
  the user's inbox. When you stop (ready, blocked, or asking the user a question),
  call `unwatch_pull_request` first. A merge or close ends the watch on its
  own.
- **"Stopped watching, could not read it"** usually means GitHub's rate limit
  is used up. Run `gh api rate_limit --jq .resources.graphql` before you
  watch again. If `remaining` is near 0, wait for `reset` with one background
  command, then watch again. Never watch again in a tight loop.

### Everywhere else: `pr-wait`

Wait with `scripts/pr-wait` from this skill's directory (use its absolute
path).

```bash
pr-wait <pr> [--repo owner/name] [--until ready|green|any-change]
```

It prints one line per real event. It exits 0 when the `--until` condition is
met or the PR merges, 1 when it needs you (a required check failed, conflicts,
unresolved threads, PR closed), and 2 on bad input.

- `ready` (script default): green checks, a Greptile or Macroscope review on
  the current head, and no unresolved threads. Use when those are the gates.
- `green`: use when waiting for required CI. With no configured required
  checks, the script treats ordinary CI failures as blocking.
- `any-change`: exit on the first event. Use for other reviews, approvals, or
  advisory CI. Evaluate readiness directly in GitHub when no CI runs.

In Claude Code, when available, run it with the Monitor tool (`timeout_ms: 3600000`), so each
line wakes you. Without a monitor, run it as one long command. It ends on its
own.

Start it again after you push, after you handle an exit 1, and after a resume.
It remembers what it already reported, so nothing that arrived in between is
lost. If it says another pid already watches the PR, do not start a second one.
Kill the old one only if its events do not reach you.

With either watcher: when an event needs no action, end your turn without a
message. Never post or say "still waiting".

## GitHub rate limits

`gh pr create`, `gh pr view`, and `gh pr checks` use GraphQL. When GraphQL is
used up, the REST limit is separate and usually still has room. Use
`gh api repos/<owner>/<repo>/pulls/<n>` to read a PR, and
`gh api repos/<owner>/<repo>/commits/<sha>/check-runs` for checks.

## Reviews

Check findings against the current head before acting, including unresolved
feedback that predates the latest push. Verify each finding against the source
before changing code. When authorized, fix real findings and CI failures.
Distinguish repository failures from infrastructure flakes. Report findings to
the user when the task is monitoring only.

If the repo has `.macroscope/check-run-agents/*.md`, read those rules before you
reply to a Macroscope finding. Its findings cite them.

Use the repository's required checks and review gates. Treat CodeRabbit as
advisory when the repository does.

For an authorized dismissal, reply with a concrete reason and resolve the
thread after verifying the reply. Use [leaving-pr-comments](../leaving-pr-comments/SKILL.md)
for GitHub replies.

Screenshots and videos help as well, when you have a way to upload them.

Keep the PR at the size the user's goal needs. Review loops tend to grow a
PR one cheap-looking fix at a time. Fix real bugs in the intended case.
Decline the rest with a written reason, and list them as follow-ups:
unneeded tests, guards for edge cases that do not matter, extra integration
or compatibility, and small performance costs. Prefer the fix that deletes or
narrows code.

## Base branch and merging

Track changes to the PR's actual base branch. `pr-wait` reports when
new base commits touch this PR's files, and when the PR becomes BEHIND or DIRTY.
`watch_pull_request` reports conflicts only, so check for BEHIND yourself
before you call the PR ready.
If an overlapping PR makes this one obsolete, stop monitoring, report it to the
user, and ask before closing the PR unless closure was explicitly authorized.

A watcher exit or event is a cue to re-read the current head, checks,
reviews, human approvals, and mergeability. If the branch needs an update,
follow [modernize](../modernize/SKILL.md) when authorized and preserve published
history. Wait for the required checks again after updating the head. In
monitoring-only mode, report any action needed. Merge only with explicit
authorization and a passing repository merge gate.
