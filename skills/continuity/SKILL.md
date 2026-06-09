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
2. If the working directory sits inside a multi-project workspace (a parent folder holding several project subfolders), treat that parent as a workspace root, not the project root. Prefer the specific project subfolder you are working in.
3. If no single project is clear, ask for the target project folder.
4. Do not write at a workspace root unless the user explicitly requests workspace-level continuity.

## Inspect First

Use fresh state. Keep inspection targeted.

For all projects, inspect when available:

- existing `.agent-continuity/CONTINUITY.md`, including its Work Log, when present
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

Never record secrets, tokens, passwords, private keys, `.env` values, credential contents, sensitive personal data, or raw sensitive media details. If a sensitive location matters for resume, record only the minimum pointer (note it under Risks And Traps) and say not to expose the secret.

Redaction overrides preservation. If a prior Work Log entry or head section already contains a secret that should never have been recorded, redact it in place — replace the value with `[redacted]` or a minimal pointer — even though entries are otherwise append-only. Note the redaction (not the value) in the new Work Log entry.

Do not enumerate, search, index, or materialize protected cloud-storage roots unless the user explicitly names that storage location and approves the scope in the current task. Protected roots include user cloud-storage mounts such as `~/Library/CloudStorage` on macOS, Proton Drive, iCloud Drive, Dropbox, Google Drive, OneDrive, and other FileProvider-backed folders.

Separate facts from assumptions. Use `Unknown`, `Not inspected`, or `Not run` instead of guessing. Do not infer completion from memory alone.

## Update Semantics

The continuity record is cumulative. Preserve history; do not overwrite it.

When updating an existing record:

1. Read the current `.agent-continuity/CONTINUITY.md` first, including its Work Log.
2. Refresh the current-state head (every section above Work Log) to reflect live state.
3. Before dropping any detail from the head, capture it in a new Work Log entry so nothing is lost.
4. Prepend one new Work Log entry for this session. Never delete, reorder, or rewrite existing Work Log entries — except to redact a secret that should never have been recorded (see Safety Rules).
5. If a prior fact or assumption is now wrong, do not erase it. Record the correction in the new entry so the history of what changed is preserved.

Keep entries concise. Never auto-prune the Work Log. Trimming or archiving old entries is a separate, explicit step the user must approve (see Continuity Audit).

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

## Work Log

Append-only. Newest entry first. Never edit or delete earlier entries.

### <local timestamp with timezone> - <short session label>

- Effort: <what this session worked on>
- Changed: <files, commands, or state touched, or `None`>
- Outcome: <result, decision, or correction to an earlier entry>

### <earlier local timestamp with timezone> - <short session label>

- Effort: <prior session summary>
- Changed: <prior session changes>
- Outcome: <prior session result>
````

## Completion Gate

Before finishing:

- Verify `.agent-continuity/CONTINUITY.md` is in the intended project.
- Read back the continuity record.
- When updating, confirm every prior Work Log entry is still present and exactly one new entry was added.
- Run a safe secret-pattern check without printing secret values. If it finds a secret in any current or prior content, redact it in place before finishing (see Safety Rules).
- Confirm explicit `Unknown`, `Not inspected`, or `Not run` markers where information is missing.

Final response: report the continuity path, what resume state was preserved, validation status, and the next action.
