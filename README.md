# claude-skills

A collection of custom [skills](https://code.claude.com/docs/en/skills) for [Claude Code](https://claude.com/claude-code).

## Installation

Clone the repo and copy (or symlink) the skill folders you want into your skills directory:

- **Per project:** `.claude/skills/` in the repo you're working in
- **Global (all projects):** `~/.claude/skills/`

```bash
git clone https://github.com/PatrizioR/claude-skills.git
cp -r claude-skills/orchestrate ~/.claude/skills/orchestrate
```

Some skills are **disabled by default** and opt-in per machine. Where a skill ships a `config.example.json`, copy it to `config.json` inside the skill folder and adjust it there. `config.json` is machine-local — never check it in.

## Skills

### orchestrate

Turns the session into an evidence-disciplined orchestrator: it owns the investigation, the plan, and the review, and delegates only fully specified mechanical steps to Opus sub-agents. One thread keeps the whole picture; the workers do the typing.

Key ideas:

- **Evidence rules** — every claim about code behavior cites `file:line` and carries a **VERIFIED** / **INFERRED** / **UNVERIFIED** label. Unverified claims are never rounded up.
- **Structured plans** — goal, findings with evidence, open questions, rejected alternatives, and ordered steps marked `DELEGATED` or `YOURS`.
- **Routing** — a repo-local skill outranks a generic worker; search-shaped questions go to a read-only `Explore` agent; only mechanical, fully specified steps go to `general-purpose` workers on Opus.
- **Skeptical review** — a sub-agent reporting success is a claim, not evidence: the orchestrator re-checks diffs and re-runs the gates itself.

**Triggers:** "orchestrate this", `TASK: ... SCOPE: ... CONSTRAINTS: ...`, "plan it and delegate the implementation". Not meant for trivial one-file edits.

**Setup:** disabled by default. Copy `orchestrate/config.example.json` to `orchestrate/config.json` and set `"disabled": false`.

| Config key | Type | Default | Meaning |
|---|---|---|---|
| `disabled` | bool | `true` | Opt-in switch; `false` lets the skill run. |
| `implementer_model` | string | `"opus"` | Model for every delegated step (`fable`/`haiku` are never valid here). |
| `max_parallel_agents` | int | `3` | Upper bound on concurrently running sub-agents. |

## License

No license yet — all rights reserved unless a `LICENSE` file is added.
