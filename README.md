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
| `experimenter` | Implements and runs experiments. Writes code, validates with a sanity test, runs in the configured environment, reports honestly. Requires an Environment section. | Read, Write, Edit, Bash, Glob, Grep | opus |

(More agents will be added as we build them out.)

## Outsourced to ARS

Some research roles are already covered by the [Academic Research Skills (ARS)](https://github.com/Imbad0202/academic-research-skills) plugin at high quality. We do not re-implement these; we call ARS directly.

| Role | ARS entry point | When to use |
|---|---|---|
| Draft a paper section | `/ars-revision` (existing draft → revised) or `/ars-plan` (Socratic outline) | Whenever a section needs to be written or substantially rewritten. |
| Write an abstract | `/ars-abstract` | Bilingual abstract + keywords. |
| Draft outline only | `/ars-outline` | Detailed outline + evidence map. |
| Citation check | `/ars-citation-check` | Audit citation correctness and format. |
| Peer-review simulation | `/ars-reviewer` | Simulated multi-perspective peer-review panel. |
| Literature review | `/ars-lit-review` | Annotated bibliography in paper format. |
| AI disclosure statement | `/ars-disclosure` | Venue-specific AI-usage statement. |
| Full research-to-publication pipeline | `/ars-full` | Research → write → review → revise → finalize. |

The orchestrator invokes these slash commands directly. The agents in this repo cover what ARS does not: critical thinking on partial work, experiment execution, visualization, fact-checking, and project-specific roles.

Install ARS:
```
/plugin marketplace add Imbad0202/academic-research-skills
/plugin install academic-research-skills
```

## Per-project configuration

Some agents require project-specific context (compute setup, conventions, file layouts). These agents have an `Environment` placeholder section in their system prompt. The user fills this in for each project, either manually or with Claude Code's help. See `experimenter.md` for the convention.

A natural setup flow:
1. Add a new project.
2. Ask Claude Code: "Help me set up the Environment section for the experimenter agent for this project."
3. Claude reads the project's existing scripts / skills / SSH config and drafts the Environment section.
4. You review and commit a `projects/<project-name>/environment.md` (or paste the Environment block into the agent invocation prompt).

## Design principles

1. **One role, one agent.** A SCHOLAR does not write. A WRITER does not run experiments. No multi-hat agents.
2. **Minimum necessary tools.** An agent that reasons but does not modify gets `Read` only. An agent that produces files gets `Write` but not `Edit`. Tool scarcity is a feature.
3. **Project-agnostic system prompts.** Project context flows in via the orchestrator's launch prompt, not via hard-coded paths in the agent definition.
4. **The orchestrator is a human (or Claude in a chat).** Agents propose; orchestrator routes and decides.

## Conventions

- All agents output markdown reports. They do not modify files unless their role specifically requires it.
- All agents respect strict scope. If asked to do something outside scope, they refuse and identify the appropriate agent.
- All agents use plain English. No metaphors, no flourish, no rhetorical hedging.
