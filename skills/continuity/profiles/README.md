# Continuity writer profiles (optional model routing)

These bundled profiles let the continuity skill delegate **ordinary** continuity logging to a strong
**one-tier-down** model at **low effort**, while keeping risky work on the stronger/current model. They
are optional: the skill is fully usable without them (see [Fallback](#fallback)).

## Profiles

| File | Target tool | Installs as |
| --- | --- | --- |
| [`continuity-writer.claude.md`](continuity-writer.claude.md) | Claude Code subagent | `~/.claude/agents/continuity-writer.md` (or project `.claude/agents/`) |
| [`continuity-writer.codex.toml`](continuity-writer.codex.toml) | Codex custom agent | `~/.codex/agents/continuity-writer.toml` |

Both define one profile named `continuity-writer` with aligned behavior.

## Install

### Claude Code

```sh
mkdir -p ~/.claude/agents
cp skills/continuity/profiles/continuity-writer.claude.md ~/.claude/agents/continuity-writer.md
```

`model: sonnet` selects the one-tier-down tier. Claude subagents take a model **tier** (`opus` /
`sonnet` / `haiku` / `inherit`); there is no per-subagent reasoning-effort field, so "low effort" is
delivered as the lower tier.

### Codex

```sh
mkdir -p ~/.codex/agents
cp skills/continuity/profiles/continuity-writer.codex.toml ~/.codex/agents/continuity-writer.toml
```

Codex custom agents take both `model` and `model_reasoning_effort`. The template ships
`model = "gpt-5.4"` with `model_reasoning_effort = "low"`; change `model` to your installed strong
one-tier-down model if `gpt-5.4` is not available. The skill's [`agents/openai.yaml`](../agents/openai.yaml)
is a Codex display/invocation interface only and does not carry model routing.

## Model-routing behavior

The skill (`SKILL.md` → **Model Routing**) delegates to `continuity-writer` **only** when the request is
a plain Continuity Log with no audit, secret-redaction, multi-root, runtime-heavy, or ambiguous-root
signal, and the profile is installed. The caller passes the session state it holds into the profile,
which then inspects live project state and writes `.agent-continuity/CONTINUITY.md` in the standard
format. Risky work stays on the stronger/current model; a profile may also self-decline with
`CONTINUITY_DECLINE: <reason>`, which sends the task back to the caller.

## Fallback

If no `continuity-writer` profile is installed, the skill runs inline on the current model with no loss
of function — routing changes only which model writes the record, never the format or the safety rules.

## Sync

The public repo is canonical. Update installed copies **from** this package (re-run the install copy
above); do not edit the installed profile or `SKILL.md` independently and let them drift. When the
package changes, re-copy the profile and re-apply the **Model Routing** section of `SKILL.md` to your
installed skill.

Verify an installed copy is in sync by diffing it against the package (run from the repo root):

```sh
# profiles
diff skills/continuity/profiles/continuity-writer.claude.md ~/.claude/agents/continuity-writer.md
diff skills/continuity/profiles/continuity-writer.codex.toml ~/.codex/agents/continuity-writer.toml
# whole skill folder (drop -q to see the lines that differ)
diff -qr skills/continuity ~/.claude/skills/continuity
```

An installed `SKILL.md` may legitimately differ from the package if you have added local-only sections
on top of it; in that case diff to confirm only your intended local additions differ, and that the
shared sections (including **Model Routing**) still match the package.

**Re-apply to every installed skill copy, not just one.** The skill commonly lives in more than one
place at once — a tool-specific dir (`~/.claude/skills/continuity/`, a Codex skills dir, `.claude/skills/`
in a project) **and** the cross-tool `~/.agents/skills/continuity/` discovery alias. A tool reads the
copy on its own search path, so routing added to one copy does not reach a session that loads another.
If `log work` keeps running on the flagship model instead of the `continuity-writer` profile, the loaded
`SKILL.md` is almost certainly a copy that still lacks the **Model Routing** section. Patch all copies.

> Note: routing is advisory, not forced. Even with the section present, a runtime may still log inline on
> the current model rather than spawn `continuity-writer`. To guarantee the cheaper path, name the
> profile explicitly (e.g. "use the continuity-writer profile to log work").
