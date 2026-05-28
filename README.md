# Research Agents

Reusable agent definitions for research projects. Each agent in `agents/` is a single-role specialist with minimum necessary tools and a strict scope.

Designed for use with Claude Code (`~/.claude/agents/`).

## Installation

Symlink the agents into your Claude Code agents directory:

```bash
git clone git@github.com:Peter00796/ResearchAgents.git ~/code/ResearchAgents
ln -sf ~/code/ResearchAgents/agents/*.md ~/.claude/agents/
```

After symlinking, the agents become available as `subagent_type` values for the `Task` tool, and (depending on the Claude Code version) may also be invokable via slash commands.

## Roster

| Agent | Role | Tools | Model |
|---|---|---|---|
| `scholar` | Critical thinker. Audits arguments, finds gaps, proposes structural revisions. | Read, Grep, Glob, WebSearch, WebFetch | opus |

(More agents will be added as we build them out.)

## Design principles

1. **One role, one agent.** A SCHOLAR does not write. A WRITER does not run experiments. No multi-hat agents.
2. **Minimum necessary tools.** An agent that reasons but does not modify gets `Read` only. An agent that produces files gets `Write` but not `Edit`. Tool scarcity is a feature.
3. **Project-agnostic system prompts.** Project context flows in via the orchestrator's launch prompt, not via hard-coded paths in the agent definition.
4. **The orchestrator is a human (or Claude in a chat).** Agents propose; orchestrator routes and decides.

## Conventions

- All agents output markdown reports. They do not modify files unless their role specifically requires it.
- All agents respect strict scope. If asked to do something outside scope, they refuse and identify the appropriate agent.
- All agents use plain English. No metaphors, no flourish, no rhetorical hedging.
