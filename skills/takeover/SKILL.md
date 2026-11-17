---
name: takeover
description: Take over existing work. Use when the user asks to take over a PR, branch, or agent thread, or requests a takeover.
---

# Takeover

Take responsibility for finishing the original goal while preserving the
original contributors' credit. Do not defend an implementation just because
another agent already built it.

Read the source PR, branch, or thread, including review findings and the user's
later corrections. Resolve the handoff to the actual code and current head,
and account for uncommitted work. Treat the handoff as context, not proof.

For a replacement PR, import the original commits onto a new branch and add
your fixes separately. Honor an explicitly requested in-place or stacked
continuation instead. Do not overwrite unrelated work or change the original
PR without authorization.

Preserve existing Git history and human contributor credit. Credit the
original human contributors in the PR description and link the original PR
when the link is safe to share. When preparing a squash message, use verified
identities for human author and co-author credit. Apply the
[file-pr](../file-pr/SKILL.md) publication rules to newly written text.

Use `modernize` to update the continuation against its intended base and
current architecture. Rework what is necessary to finish the same goal, not
unrelated features. Simplify the implementation without dropping intended
behavior or surfaces. Recheck the source PR too: it may have improved while
you were working.

Independently verify the original failure and realistic workflows, following
repository guidance and permissions. Green mocked tests do not prove the user
experience works. State what remains unverified.

When the requested finish includes a PR, use `file-pr` to file or update it,
then `babysit-pr` rather than stopping at creation. Use `leaving-pr-comments`
for replies. Merge or close the original PR only when authorized.

Report what you inherited, what you changed, how you verified it, how credit
was preserved, and any remaining decision.
