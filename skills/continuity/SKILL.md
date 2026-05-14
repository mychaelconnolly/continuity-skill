---
name: continuity
description: Use when the user asks to preserve work state for later, including "continuity", "continuity log", "log work", "save state", "continue later", "resume work", "resume note", "session clearing", "history clearing", "memory clearing", or "save critical context". Create concise project-local continuity records so a future agent session can resume accurately.
---

# Continuity

Create or update project-local continuity records for future agent sessions.

## Output

Default output: `.agent-continuity/CONTINUITY.md` in the active project. Create `.agent-continuity/` if needed.

Do not create scripts, templates, README files, changelogs, or auxiliary files for ordinary continuity work. If related notes already exist, read them as context when useful, but write the continuity record to `.agent-continuity/CONTINUITY.md`.

## Modes

### Continuity Log

Default mode for "save state", "continue later", "resume work", "log work", and similar requests.

Goal: record the minimum verified state needed for a future agent session to continue without rediscovery.

### Continuity Audit

Use only when the user asks to prepare for agent session, history, or memory clearing.

Create an audit section in `.agent-continuity/CONTINUITY.md` before any deletion step. Inventory project-critical resume context from available local records, classify each source, and migrate only what is needed for project continuity. Do not delete anything unless the user explicitly approves that separate destructive step after the clearance note.

### Runtime Continuity

Use when work depends on running processes, dev servers, remote hosts, services, ports, URLs, state files, or external runtime boundaries.

Record the local project path plus runtime state, read/write boundaries, status files, resume commands, and stop/cleanup commands only when already known or verified.

## Project Root

Choose the root conservatively:

1. If inside a Git repo, use `git rev-parse --show-toplevel`.
2. Treat `/Users/michael/codex` as a workspace root, not a project root.
3. Prefer direct child projects under `/Users/michael/codex/<project>`.
4. If no single project is clear, ask for the target project folder.
5. Do not write at `/Users/michael/codex` unless the user explicitly requests workspace-level continuity.

## Inspect First

Use fresh state. Keep inspection targeted.

For all projects, inspect when available:

- `pwd`
- applicable `AGENTS.md` instructions
- README or key local notes relevant to resuming work
- manifests and obvious test/build commands
- files, logs, state artifacts, or runtime notes relevant to the current task
- commands already run in the session and their results

For Git projects, also inspect:

- `git status --short --branch`
- `git diff --stat` and focused relevant diffs
- branch name and recent commits when they affect resume or publishing state
- remotes, `gh auth status`, dry-run push, PR, and check state when publishing is relevant

For local folders without Git, inspect:

- shallow directory shape
- recently changed files by timestamp when useful
- available validation commands from project files

For agent runtime state, record when relevant:

- sandbox mode, writable roots, and escalation constraints
- running `exec_command` sessions, dev servers, URLs, ports, and process status
- user-approved plans, tools, repos, packages, and no-pivot constraints

## Safety Rules

Never record secrets, tokens, passwords, private keys, `.env` values, credential contents, sensitive personal data, or raw sensitive media details. If a sensitive location matters for resume, record only the minimum pointer and say not to expose the secret.

Do not enumerate, search, index, or materialize protected cloud-storage roots unless the user explicitly names that storage location and approves the scope in the current task. Protected roots include `/Users/michael/Library/CloudStorage`, Proton Drive, iCloud Drive, Dropbox, Google Drive, OneDrive, and other FileProvider-backed folders.

Separate facts from assumptions. Use `Unknown`, `Not inspected`, or `Not run` instead of guessing. Do not infer completion from memory alone.

## Continuity Format

Use concise Markdown:

````md
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
````

## Completion Gate

Before finishing:

- Verify `.agent-continuity/CONTINUITY.md` is in the intended project.
- Read back the continuity record.
- Run a safe secret-pattern check without printing secret values.
- Confirm explicit `Unknown`, `Not inspected`, or `Not run` markers where information is missing.

Final response: report the continuity path, what resume state was preserved, validation status, and the next action.
