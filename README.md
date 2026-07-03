<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/continuity-mark-dark.svg">
  <img src="assets/continuity-mark-light.svg" alt="⋯" width="240">
</picture>

# continuity

**Manual project memory for coding agents.**

Ask your agent to "log work" — get a durable, human-readable resume note that lives with your project.

[![Built to the Agent Skills spec](https://img.shields.io/badge/Agent_Skills-spec-5436DA)](https://agentskills.io/specification) [![Follows Anthropic Skill best practices](https://img.shields.io/badge/Anthropic-best_practices-D97757?logo=anthropic&logoColor=white)](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) [![Ships an OpenAI Codex interface](https://img.shields.io/badge/OpenAI_Codex-skills-412991?logo=openai&logoColor=white)](https://developers.openai.com/codex/skills) [![License: MIT](https://img.shields.io/badge/License-MIT-3DA639)](LICENSE)

</div>

---

`continuity` is a small [Agent Skill](https://agentskills.io/specification) that writes a project-local memory note at `.agent-continuity/CONTINUITY.md` — on demand, never in the background. It is intentionally boring: one `SKILL.md`, one Markdown output file, no service, no database, no background capture.

## What It Leaves Behind

```md
# Continuity

Last updated: <timestamp>
Project: <absolute path>

## Current Goal
## Current State              ← facts vs assumptions, unknowns marked
## Changed Or Relevant Files
## Commands And Results
## Tests And Validation
## Runtime State
## Decisions And Constraints
## Risks And Traps
## Next Action
## Resume Prompt              ← paste into the next session, any tool
## Work Log                   ← append-only, newest entry first
```

The record is cumulative: each run refreshes the head and prepends a dated work-log entry — history is never deleted. See a [full example record](examples/CONTINUITY.example.md).

## Quick Start

```sh
# Claude Code — then invoke with /continuity or ask to "log work"
cp -R skills/continuity ~/.claude/skills/continuity

# Gemini CLI
gemini skills link ./skills/continuity
```

- **Codex** — install from [`skills/continuity`](https://github.com/mychaelconnolly/continuity-skill/tree/main/skills/continuity), or copy that folder into your Codex skills directory.
- **Anything else** — paste [`adapters/generic-prompt.md`](adapters/generic-prompt.md) into the agent's custom instructions.

Then, at any natural stopping point:

```text
log work
```

And in any future session, with any tool:

```text
Resume this project. Read `.agent-continuity/CONTINUITY.md` first, then inspect live state before making changes.
```

## More

- **[Overview & details](docs/OVERVIEW.md)** — why manual memory, what gets recorded, ecosystem fit, safety model, and the standards this is built to.
- **[Model routing (optional)](docs/OVERVIEW.md#model-routing-optional)** — run ordinary logging on a cheaper one-tier-down model via the bundled `continuity-writer` profiles.
- **[Full example record](examples/CONTINUITY.example.md)** — a complete `.agent-continuity/CONTINUITY.md`.

## License

MIT.
