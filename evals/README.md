# Evaluations

Scenarios for checking that the `continuity` skill behaves correctly. Each scenario in
[`scenarios.md`](scenarios.md) uses the Agent Skills evaluation shape: a `query` the agent
receives, an optional `setup` describing the starting state, optional input `files`, and an
`expected_behavior` list to grade against.

There is no built-in runner for these. Per Anthropic's
[skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices),
evaluation harnesses are user-provided. Use the scenarios two ways:

- **Manual:** reproduce a scenario's `setup` in a scratch project, run the skill, then check the
  output against each `expected_behavior` item.
- **Automated:** feed each scenario to your own grader (an LLM judge or assertions) and score the
  `expected_behavior` items as pass or fail.

The most important scenario is `preserve-history-on-update`: it guards the rule that updating an
existing record must refresh the current-state head while never deleting earlier Work Log
entries.
