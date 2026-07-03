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

One note, this shape — filled in here from a fictional but faithful session:

> ### Continuity
>
> Last updated: 2026-07-02 09:05 EDT \
> Project: `~/lumon/mdr-refinement` \
> Branch: `cold-harbor`
>
> #### Current Goal
>
> Refine the Cold Harbor file to 100% before quarter end.
>
> #### Current State
>
> ##### Facts
>
> - `refine.py` bins the scary numbers; 96% complete (verified: `./refine --status`)
>
> ##### Assumptions
>
> - The work is mysterious and important. Purpose of the bins: Unknown.
>
> #### Changed Or Relevant Files
>
> - `bins/wellness.json` - four tempers rebalanced
>
> #### Commands And Results
>
> - `./refine --file cold_harbor` - exit 0, 96% → 97%
>
> #### Tests And Validation
>
> - `pytest tests/tempers` - passed (4/4: WO, FC, DR, MA)
>
> #### Runtime State
>
> - macrodata terminal on port 5309 - running
>
> #### Decisions And Constraints
>
> - Board approved the refinement plan; no cross-department pivots without check-in
>
> #### Risks And Traps
>
> - Never prune the Work Log. Memory does not survive the elevator; this file does.
>
> #### Next Action
>
> Run `./refine --file cold_harbor --final`, then request the waffle party.
>
> #### Resume Prompt
>
> Resume work in ~/lumon/mdr-refinement. Read `.agent-continuity/CONTINUITY.md`
> first, then inspect live state before making changes.
>
> #### Work Log
>
> *Append-only, newest entry first.*
>
> ##### 2026-07-02 09:04 EDT - cold harbor push
>
> - Effort: binned the remaining scary numbers
> - Outcome: 97%; one session from done
>
> ##### 2026-07-01 16:40 EDT - session severed
>
> - Effort: refinement paused at 96%
> - Outcome: state preserved for the next innie

The record is cumulative: each run refreshes the head and prepends a dated work-log entry — history is never deleted. See a [full example record](examples/CONTINUITY.example.md).

## Quick Start

Point your agent at the repo:

```text
Install the continuity Agent Skill from
https://github.com/mychaelconnolly/continuity-skill — copy skills/continuity
into your skills directory and reload skills.
```

Or manually — same move everywhere, only the destination differs:

```sh
git clone https://github.com/mychaelconnolly/continuity-skill
cp -R continuity-skill/skills/continuity <your-skills-dir>
# e.g. ~/.claude/skills, ~/.codex/skills, ~/.agents/skills, .gemini/skills
```

If your tool does not discover Agent Skills at all, paste [`adapters/generic-prompt.md`](adapters/generic-prompt.md) into its custom instructions instead. Per-tool notes live in [docs/OVERVIEW.md](docs/OVERVIEW.md#install).

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
