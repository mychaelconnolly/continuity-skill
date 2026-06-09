# Generic Continuity Instructions

Use these instructions in agents that do not discover Agent Skills natively.

> Canonical source: `skills/continuity/SKILL.md`. This adapter keeps the format inline on purpose, because it targets tools without skill or file discovery. Keep the two in sync when either changes.

When the user asks to preserve work state for later, such as "log work", "save continuity", "continue later", "resume work", "session clearing", "history clearing", or "save critical context", create or update `.agent-continuity/CONTINUITY.md` in the active project.

Treat this file as both agent memory and human-readable project documentation. It should let a future agent, model, or human resume work without relying on hidden conversation history.

## Behavior

1. Read the existing `.agent-continuity/CONTINUITY.md`, including its Work Log, before writing. Then inspect current state with fresh facts from the project, not memory alone.
2. Choose the project root conservatively. If inside a Git repo, use the repo root. Do not write at a workspace root unless the user explicitly asks for workspace-level continuity.
3. Record only verified state. Mark gaps as `Unknown`, `Not inspected`, or `Not run`.
4. Separate facts from assumptions.
5. Include the next action as a single concrete step.
6. Preserve history. The record is cumulative: refresh the current-state head, but prepend a new dated Work Log entry and never delete, reorder, or rewrite earlier entries. If a prior fact is now wrong, record the correction in the new entry instead of erasing it. Secrets are the one exception: if a prior entry already contains one, redact it in place rather than preserving it.
7. Do not record secrets, tokens, passwords, private keys, raw credential values, or sensitive personal data. If a sensitive location matters for resume, record only a minimal pointer under Risks And Traps and say not to expose the secret.
8. Read the record back before finishing and run a safe secret-pattern check without printing secret values; if the check finds a secret anywhere in the record, redact it in place before finishing.

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

## Work Log

Append-only. Newest entry first. Never edit or delete earlier entries.

### <local timestamp with timezone> - <short session label>

- Effort: <what this session worked on>
- Changed: <files, commands, or state touched, or `None`>
- Outcome: <result, decision, or correction to an earlier entry>
```
