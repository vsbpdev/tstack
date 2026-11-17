---
name: full-stop
description: Use when the user explicitly asks to "full stop" a task by preparing a merge-ready PR for them. Ordinary use of "full stop" as emphasis does not invoke it.
---

# Full Stop

Follow [full-send](../full-send/SKILL.md) through its readiness check and
leave the PR open for the user to merge. Repository ownership, publication,
testing, and review gates apply throughout.

Keep published history intact. Never merge, enable auto-merge, enter a merge
queue, push to the default branch, or delegate those actions. Check whether
existing auto-merge or a queue could merge this PR before you update it. If
you are not authorized to disable it, report the blocker before updating the
PR. If you are authorized and disable it, confirm the result before pushing.

Fetch the target branch and read the live PR head, base, checks, reviews,
and mergeability. Report whether it is ready, waiting for a required approval,
blocked by another gate, or already merged by someone else. Include the PR
link, final head, checks run, and remaining decisions. In T3 Code, unwatch the
PR before handing it off.
