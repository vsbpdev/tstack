---
name: full-send
description: Use when the user explicitly asks to "full send" a change through review, PR checks, and an authorized merge.
---

# Full Send

Complete the task through the furthest step the user and repository allow.
An explicit full-send request includes the ordinary steps to file and merge
an authorized PR. Repository ownership, test permissions, required reviews,
publication rules, and human approval gates still apply. Use
[full-stop](../full-stop/SKILL.md) for a merge-ready handoff.

## Steps

1. **Finish the change.** Complete the requested behavior with the smallest
   change that works. Use a task branch and preserve unrelated work.
2. **Verify it.** Run the project's relevant checks. Reproduce the original
   failure and confirm the fix when practical. Use the normal runtime for
   user-facing behavior when available and authorized. Record any material
   verification gap.
3. **Review it.** Use the project's required review workflow when defined.
   For a standalone change, ask a fresh reviewer to inspect the diff and
   focus on correctness. Prefer Codex for Claude-authored changes via
   [sol-loop](../sol-loop/SKILL.md), and a configured Claude reviewer for
   Codex-authored changes. Use the project's authorized dispatch route.
   Address material findings and recheck affected behavior. If the configured
   review cannot run, identify the missing result and respect the review gate.
4. **File the PR** with [file-pr](../file-pr/SKILL.md) when publishing is
   authorized. Check the target branch and commit only the task's changes.
   Update the topic branch from its base if required, using a merge where the
   repository permits it. Recheck behavior affected by the update. Push only
   the authorized topic branch and keep published history intact.
5. **Babysit the PR** with [babysit-pr](../babysit-pr/SKILL.md).
   Verify each finding and CI failure before changing code. Recheck affected
   behavior after fixes.
6. **Merge** only when authorized and the merge gate below passes.
7. **Report** the PR link, the merge commit or current head, the checks
   actually run, and any remaining decisions or verification gaps.

Stay with the task through the authorized finish. Use the PR watcher to wait
for changes. Before yielding, record the PR URL, current head, last completed
step, and pending gate in the project run record or private work summary. Read
that record and check live state on wake. Unwatch before handing off or asking
for approval.

## Merge gate

Confirm the current PR head, intended base, and GitHub merge state. Confirm
that the verification covers the current behavior, required checks pass,
required approvals and review feedback are addressed, and relevant base
changes are accounted for. Apply repository policy to advisory checks.
Follow native branch protections and use the repository's merge method.
If an authorized merge enters a queue, follow it until GitHub reports the
actual outcome.

When a required test, review, or approval is unavailable, finish independent
work and report the PR state and the exact gate still open. Ask only when
the next step requires the user's decision or authorization.
