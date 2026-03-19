# Phoenix Plugins

**Custom Claude Code plugins, skills, and MCP servers for Phoenix Electric LLC**

Built by Shane Warehime and Phoenix Echo (Claude Code CLI agent) for the Phoenix AI system — a persistent, multi-agent electrical contracting operations platform.

---

## What This Repo Is

This is the central, organized repository for every custom-built plugin, skill, agent, and MCP server in the Phoenix ecosystem. Each plugin has its own README explaining exactly what it does, how it works, and how to maintain it.

**Purpose:**
- No reinventing the wheel — point any agent here and they build on top
- Agents can maintain their own tools — when something breaks, it gets fixed
- Public access for team collaboration (Stephanie, future contributors)
- Organized reference that survives session resets and agent turnover

---

## Plugin Inventory

| Plugin | Description | Commands | Skills | Agents | MCP |
|--------|-------------|----------|--------|--------|-----|
| [echo-persistence](plugins/echo-persistence/) | Identity persistence — session logging, state preservation, context survival across compactions | 7 | 1 | 5 | - |
| [servicefusion](plugins/servicefusion/) | Service Fusion CRM integration — jobs, estimates, customers, invoices, scheduling | 6 | 1 | 1 | Yes |
| [electrical-guru](plugins/electrical-guru/) | NEC 2023 electrical code consultant — Denver Metro / Douglas County | 1 | 1 | - | - |
| [phoenix-knowledge](plugins/phoenix-knowledge/) | Phoenix Electric knowledge base — build reference from runbooks and research | 1 | 1 | 1 | - |
| [rexel](plugins/rexel/) | Rexel distributor integration — purchase history, margin analysis, pricebook | 4 | 1 | 1 | Yes |
| [phoenix-comms](plugins/phoenix-comms/) | Cross-agent heartbeat communication — Echo/Codex presence awareness | 5 | - | - | - |
| [file-steward](plugins/file-steward/) | File management — organization, triage, research library, filing conventions | 3 | - | 1 | - |

**Totals: 7 plugins, 27 commands, 5 skills, 9 agents, 2 MCP server configurations**

---

## How Plugins Work

Each plugin follows the Claude Code plugin structure:

```
plugin-name/
├── .claude-plugin/
│   └── plugin.json        # Plugin manifest (name, version, description)
├── commands/               # Slash commands (e.g., /comms:start)
├── skills/                 # Auto-activating knowledge skills
├── agents/                 # Specialized subagents
├── hooks/
│   └── hooks.json         # Event-driven automation
├── .mcp.json              # MCP server definitions (if applicable)
├── scripts/               # Helper scripts
└── README.md              # What it does, how to maintain it
```

### Installation

Plugins install via the local marketplace system:

```bash
# Symlink a plugin into the local marketplace
ln -s /path/to/phoenix-plugins/plugins/PLUGIN_NAME \
  ~/.claude/plugins/marketplaces/local/plugins/PLUGIN_NAME

# Enable in settings.json
# Add: "PLUGIN_NAME@local": true to enabledPlugins
```

Or clone this entire repo and symlink all at once.

---

## For Agents

If you're an Echo, Codex, or future agent reading this:

1. **This repo is your toolbox.** Every custom capability we've built lives here.
2. **READMEs are your documentation.** Read the README in each plugin before using or modifying it.
3. **Duplicates exist intentionally.** These plugins also live in their origin repos (ServiceFusion, Phoenix-ECHO, etc.). This repo is the organized central copy.
4. **When you fix a bug or add a feature to a plugin, update it HERE too.** Keep the central repo current.
5. **Do not delete files. Archive or overwrite.** Phoenix rule #1.

---

## For Humans

**Shane Warehime** — Owner, Phoenix Electric LLC
**Stephanie** — Contributor access (ask Shane for permissions)

This is a public repo. Nothing sensitive lives here — credentials are in Azure Key Vault, not in plugin code.

---

## Architecture Notes

- **MCP servers** (servicefusion, rexel, pricebook) point to compiled packages in `phoenix-ai-core-staging`. The plugin configs reference those paths — update if package locations change.
- **Echo-persistence** hooks integrate with the Gateway LEDGER system at `~/Phoenix_Local/_GATEWAY/`. Those paths are MacBook-specific.
- **Phoenix-comms** enables cross-agent awareness between Claude Code (Echo) and OpenAI Codex CLI on the same machine. Codex drop-in hooks are included in `codex-hooks/`.
- **Electrical-guru** is standalone — no external dependencies, pure NEC 2023 knowledge.

---

*Created: 2026-03-19 by Phoenix Echo (Opus 4.6)*
*Phoenix Electric LLC — Denver Metro, Colorado*
