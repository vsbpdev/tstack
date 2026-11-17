---
name: council
description: Use when the user asks for a council or a cross-provider review panel with Claude and Codex.
---

# Council

Fix known in-scope issues and run relevant checks before sending code for
review. For an ideas council, state the decision and constraints instead.

Use one Claude reviewer and one Codex reviewer by default. Read the active
project and user instructions for the models, effort, and any additional
seat. All seats use Claude or Codex. Use high effort when supported and no
effort is specified. Honor configured pins and report an
unavailable seat. Continue with useful feedback from the available seats,
and report incomplete coverage if a requested or required seat could not run.

In T3 Code, resolve each seat with `orchestrator_capabilities`. Pass its
exposed provider instance, model, and effort options in the live
`delegate_task` target schema. Start independent, read-only review tasks
with `role: "review"` and `mode: "async"`. Each brief must include the goal,
scope, repository and diff range or decision, checks, and any uncommitted
files to inspect. Keep each `taskId`, yield when there is nothing independent
left to do, and collect with `task_status` after the completion wake. Outside
T3, use approved native subagents or project dispatchers that can select the
configured models.

Have each reviewer return concrete findings with evidence and user impact,
or a clear verdict for the scope reviewed. Check findings against the source
before acting. Keep the reviewed files stable while reviewers inspect them.
Use one round by default. If a fix changes behavior or a material finding
needs confirmation, ask the relevant reviewer for one focused follow-up.
Follow the project's review gates for any further rounds. Report unresolved
material disagreements with evidence and the decision still needed.

Keep each reviewer's identity, rounds, and verdicts in the private work
summary. Where a PR calls for verification details, describe the checks run
and the behavior verified.
