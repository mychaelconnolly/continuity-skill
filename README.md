# continuity-skill

Manual project memory for coding agents.

`continuity` is a small Agent Skill that writes a durable project-local memory note at `.agent-continuity/CONTINUITY.md` when you ask for it. It gives the current agent a clean resume point, gives future humans readable project documentation, and keeps context with the project folder even if you switch sessions, machines, agents, or tools.

It is intentionally boring: one `SKILL.md`, one Markdown output file, no service, no database, no background capture.

## Why This Exists

I made this in May 2026, while agent memory is still moving quickly across Codex, Claude, Gemini, Grok-backed tools, MCP memory servers, and stateful-agent frameworks. I expect agentic memory to become more native, consent-aware, auditable, project-scoped, and portable soon.

While that settles, I still want a manual switch:

- "log work"
- "save continuity"
- "prepare for session clearing"
- "write down what the next agent needs"

The result is both agent memory and project documentation. The agent gets the verified state it needs to resume. I get a Markdown record I can inspect, edit, commit or ignore, and carry with the project.

## What It Does

When invoked, the skill tells the agent to inspect current state and create or update:

```text
.agent-continuity/CONTINUITY.md
```

The record captures:

- current goal
- verified state and assumptions
- relevant files
- commands already run and results
- tests and validation
- runtime state when relevant
- decisions, constraints, risks, and next action
- a resume prompt for the next session or next tool
- an append-only work log of past sessions
- a secrets note

The record is cumulative. Each run refreshes the current-state head and prepends a new dated work-log entry; it never deletes earlier entries. History is preserved across updates, so you accumulate a record of efforts as they are made, while the top of the file stays a clean resume point.

This is useful when you want memory you can see and control.

## Why It Is Portable

The skill uses the Agent Skills convention: a folder with `SKILL.md` and YAML frontmatter. That makes it plug-and-play in tools that support Agent Skills, and easy to adapt for tools that do not.

- Codex can install the skill directly.
- Claude Code can use the same `SKILL.md` structure.
- Gemini CLI can install or link Agent Skills and also discovers `.agents/skills` aliases.
- Grok-backed or other coding agents can use the generic prompt adapter in `adapters/generic-prompt.md`.

The most important output is not vendor-specific. `.agent-continuity/CONTINUITY.md` is plain Markdown inside your project folder.

## Install

### Codex

Install from the skill folder after this repo is published:

```text
https://github.com/mychaelconnolly/continuity-skill/tree/main/skills/continuity
```

For a local install, copy `skills/continuity` into your Codex skills directory, then start a new session or reload skills according to your Codex setup.

### Claude Code

Personal skill:

```sh
mkdir -p ~/.claude/skills
cp -R skills/continuity ~/.claude/skills/continuity
```

Project skill:

```sh
mkdir -p .claude/skills
cp -R skills/continuity .claude/skills/continuity
```

Invoke it directly with `/continuity`, or ask Claude to log continuity when the description matches your request.

### Gemini CLI

Link a local checkout:

```sh
gemini skills link ./skills/continuity
```

Or copy it into a project:

```sh
mkdir -p .gemini/skills
cp -R skills/continuity .gemini/skills/continuity
```

Gemini CLI also documents `.agents/skills` as a discovery alias, so the same folder can be shared with other Agent Skills clients.

### Grok-Backed Agents And Other Tools

If your tool does not discover Agent Skills, use the generic adapter:

```text
adapters/generic-prompt.md
```

Paste it into the agent's project instructions or custom prompt area. The behavior is the same: create or update `.agent-continuity/CONTINUITY.md` when explicitly asked.

## Usage

Examples:

```text
Use $continuity to log work before I clear this session.
```

```text
Save continuity for this project. Include the current goal, changed files, tests run, risks, and next action.
```

```text
Prepare for session clearing. Write the project continuity note first.
```

Future resume prompt:

```text
Resume this project. Read `.agent-continuity/CONTINUITY.md` first, then inspect live state before making changes.
```

## Ecosystem Fit

This skill is not a replacement for larger memory systems. It fills a narrower gap.

| Category | Examples | What they are good at | Where continuity fits |
| --- | --- | --- | --- |
| Native agent memory | Codex Memories, Gemini Auto Memory | Automatic recall and experience-level personalization | Explicit project-local notes you can inspect |
| MCP or cloud memory | Mem0, OpenMemory, Supermemory | Cross-app memory, semantic recall, hosted or configured stores | No service dependency, plain Markdown |
| Stateful-agent frameworks | Letta, LangGraph/LangMem, Zep/Graphiti | Building applications with persistent agent state | Lightweight workflow for local coding projects |
| Project docs | README, status docs, issue comments | Human-facing durable context | Standard resume shape for agents and humans |

Use this when you prefer an intentional memory action instead of background capture.

## Safety Model

The skill instructs the agent not to record secrets, tokens, passwords, private keys, raw credential values, or sensitive personal data. It also tells the agent to mark unknowns explicitly instead of guessing.

Because the output is Markdown, you can review it before sharing, committing, or using it with another tool.

## Project Layout

```text
continuity-skill/
├── README.md
├── LICENSE
├── adapters/
│   └── generic-prompt.md
├── evals/
│   ├── README.md
│   └── scenarios.md
├── examples/
│   └── CONTINUITY.example.md
└── skills/
    └── continuity/
        ├── SKILL.md
        └── agents/
            └── openai.yaml
```

## References

- [Agent Skills specification](https://agentskills.io/specification)
- [OpenAI Codex Skills](https://developers.openai.com/codex/skills)
- [OpenAI Codex Memories](https://developers.openai.com/codex/memories)
- [OpenAI Codex feature maturity](https://developers.openai.com/codex/feature-maturity)
- [OpenAI skills catalog](https://github.com/openai/skills)
- [Claude Code skills](https://code.claude.com/docs/en/skills)
- [Gemini CLI Agent Skills](https://geminicli.com/docs/cli/tutorials/skills-getting-started/)
- [Gemini CLI Auto Memory](https://geminicli.com/docs/cli/auto-memory/)
- [xAI Grok Code Fast](https://docs.x.ai/developers/models/grok-code-fast-1)
- [Mem0 MCP](https://docs.mem0.ai/platform/mem0-mcp)
- [OpenMemory](https://mem0.ai/openmemory)
- [Supermemory MCP](https://supermemory.ai/docs/supermemory-mcp/introduction)
- [Letta stateful agents](https://docs.letta.com/guides/core-concepts/stateful-agents)
- [LangChain memory](https://docs.langchain.com/oss/python/concepts/memory)
- [Zep Graphiti](https://help.getzep.com/graphiti/getting-started/overview)

## License

MIT.
