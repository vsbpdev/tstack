# tstack

Skills for reviewing and shipping changes in Claude Code and Codex. The review
and PR monitoring skills use [T3 Code](https://github.com/pingdotgg/t3code)
tools when the current thread exposes them.

## Skills

| Skill | What it does |
| --- | --- |
| `full-send` | Verifies, reviews, files, and monitors a PR through an authorized merge. |
| `full-stop` | Prepares a PR for the user to merge. |
| `council` | Gets an independent Claude and Codex review, with optional configured seats. |
| `sol-loop` | Runs a bounded Codex Sol review and rechecks substantive fixes. |
| `file-pr` | Files a concise PR with verification details. |
| `takeover` | Continues a PR, branch, or thread and preserves human contributor credit. |
| `babysit-pr` | Monitors CI and review feedback on the current PR head. |
| `modernize` | Decides whether an old PR still makes sense and updates it when authorized. |
| `leaving-pr-comments` | Writes technical GitHub replies and review comments. |

## Install

Clone the adapted branch:

```bash
git clone --branch workflow-adaptation https://github.com/vsbpdev/tstack.git
```

Link the skills you want into `.agents/skills/` for Codex or `.claude/skills/` for Claude Code at
the project level. Use the discovery path configured for the project on the T3
host. Check for existing names before linking. Keep linked skill directories
together so their relative links work.

- `full-stop` uses `full-send`.
- `full-send` uses `sol-loop`, `file-pr`, and `babysit-pr`.
- `takeover` uses `modernize`, `file-pr`, `babysit-pr`, and `leaving-pr-comments`.
- `file-pr`, `babysit-pr`, and `modernize` can call each other during PR handoff.
  Include `leaving-pr-comments` for GitHub replies.

Use authenticated `gh` for GitHub actions and Python 3 for the bundled
`pr-wait` fallback. In T3 Code, check `orchestrator_capabilities` before
delegating. Outside T3 Code, use the configured native review tools or
dispatchers.

## Project settings

Set your council seats, permitted models, reasoning effort, and approved
review route in the target project's `AGENTS.md` or `CLAUDE.md`. Keep those
settings outside the installed skills so updates preserve them. For example:

```markdown
## tstack
- Council seats: one Claude reviewer and one Codex reviewer.
- Pin the approved model and effort per seat.
- Use the configured Codex Sol reviewer for sol-loop.
- Follow this repository's test, publication, and merge approvals.
```

Public GitHub text covers the change, evidence, and human contributor credit
where due. Record model selections and review receipts in the private work
summary.

## License

[MIT](LICENSE)
