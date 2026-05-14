# Generic Continuity Instructions

Use these instructions in agents that do not discover Agent Skills natively.

When the user asks to preserve work state for later, such as "log work", "save continuity", "continue later", "resume work", "session clearing", "history clearing", or "save critical context", create or update `.agent-continuity/CONTINUITY.md` in the active project.

Treat this file as both agent memory and human-readable project documentation. It should let a future agent, model, or human resume work without relying on hidden conversation history.

## Behavior

1. Inspect current state before writing. Use fresh facts from the project, not memory alone.
2. Choose the project root conservatively. If inside a Git repo, use the repo root. Do not write at a workspace root unless the user explicitly asks for workspace-level continuity.
3. Record only verified state. Mark gaps as `Unknown`, `Not inspected`, or `Not run`.
4. Separate facts from assumptions.
5. Include the next action as a single concrete step.
6. Do not record secrets, tokens, passwords, private keys, raw credential values, or sensitive personal data.
7. Read the note back before finishing and run a safe secret-pattern check without printing secret values.

## Output Path

```text
.agent-continuity/CONTINUITY.md
```

Create `.agent-continuity/` if needed.

## Output Shape

```md
# Continuity

Last updated: <local timestamp with timezone>
Project: `<absolute path>`
Type: <Git repo | local folder | remote/runtime | mixed>
Branch: `<branch>` or `N/A`

## Current Goal

<one or two sentences>

## Current State

### Facts

- <verified fact>

### Assumptions

- <assumption, or `None recorded`>

## Changed Or Relevant Files

- `<path>` - <why it matters>

## Commands And Results

- `<command>` - <result or status>

## Tests And Validation

- <check> - <passed/failed/not run and evidence>

## Runtime State

- <service, process, URL, port, status file, or `N/A`>

## Decisions And Constraints

- <decision, approval, constraint, or `None recorded`>

## Risks And Traps

- <pitfall, boundary, stale assumption, or `None known`>

## Next Action

<single next action, with command/path when useful>

## Resume Prompt

```text
Resume work in <project path>. Read `.agent-continuity/CONTINUITY.md` first, then inspect live state before making changes. Current goal: <goal>. Next action: <next action>.
```

## Secrets Note

- No secrets, tokens, passwords, private keys, credential contents, or sensitive personal data were intentionally recorded.
```
