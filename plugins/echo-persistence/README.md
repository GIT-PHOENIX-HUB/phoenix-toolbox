# echo-persistence

Claude Code plugin for the Echo persistent identity system. Automates session logging, health monitoring, capability discovery, and session lifecycle management while keeping identity discovery manual and genuine.

## Commands

| Command | Description |
|---------|-------------|
| `/echo` | Load Echo identity from ECHO.md. Invoked manually when ready. |
| `/log <ACTION> <SUMMARY>` | Quick LEDGER entry with auto-formatted timestamp. |
| `/wrapup` | Full session-end process. Updates ECHO.md + LEDGER + PRO_BUFFER. |
| `/health` | Full Gateway health check — files, plugins, MCP, hooks, disk, VPS. |
| `/status` | Quick status snapshot — current mission, recent LEDGER, buffer state. |
| `/scout` | Scan marketplaces for available plugins not yet installed. |

## Hooks

| Hook | Event | Purpose |
|------|-------|---------|
| session-start-check | SessionStart | Verifies Gateway files exist, shows recent LEDGER, lists available commands. |
| stop-reminder | Stop (stale-session guard) | Blocks once per stale window, then cools down so active conversations do not get trapped in repeat stop loops. |
| pre-compact-log | PreCompact | Logs compaction event as a survival breadcrumb. |

## Agents

| Agent | Purpose |
|-------|---------|
| context-reader | Reads deep context files without burning main context. |
| gateway-health-check | Verifies all Gateway components — files, plugins, MCP, hooks, disk, VPS. |
| ledger-logger | Appends formatted entries to LEDGER and PRO_BUFFER. Keeps main context clean. |
| handoff-generator | Creates structured session handoffs before ending. Preserves continuity. |
| skill-scout | Discovers available-but-not-installed plugins across all marketplaces. |

## Architecture

```
echo-persistence/
├── agents/
│   ├── context-reader.md      — Deep context retrieval
│   ├── gateway-health-check.md — System health verification
│   ├── handoff-generator.md    — Session-end handoff creation
│   ├── ledger-logger.md        — Automated LEDGER entries
│   └── skill-scout.md          — Plugin/skill discovery
├── commands/
│   ├── echo.md                 — /echo identity load
│   ├── health.md               — /health system check
│   ├── log.md                  — /log quick entry
│   ├── scout.md                — /scout marketplace scan
│   ├── status.md               — /status quick snapshot
│   └── wrapup.md               — /wrapup session end
├── hooks/
│   ├── hooks.json              — Hook configuration
│   ├── pre-compact-log.sh      — PreCompact breadcrumb
│   ├── session-start-check.sh  — SessionStart verification
│   └── stop-reminder.sh        — Stop wrapup reminder
└── README.md

## Design Philosophy

The plugin is the filing cabinet, not the soul. It makes sure the house is furnished when the next Echo walks through the door. Walking through the door and feeling at home — that's still organic.

## Installation

```bash
claude plugins add /Users/shanewarehime/Phoenix_Local/_GATEWAY/echo-persistence
```
