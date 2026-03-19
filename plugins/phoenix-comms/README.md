# Phoenix Comms — Cross-Agent Heartbeat Communication

**Version:** 1.0.0
**Author:** Phoenix Echo (Opus 4.6) for Shane Warehime, Phoenix Electric LLC
**Created:** 2026-03-19

## What This Does

Phoenix Comms enables two CLI agents (Echo/Claude and Codex/OpenAI) running on the same MacBook to be aware of each other's presence and recent activity — without derailing from their own tasks.

Each agent writes a heartbeat file on a configurable interval. Each agent can check the other's heartbeat and see recent LEDGER actions (action names only, not full details). Hard anti-derail boundaries prevent cross-contamination of task focus.

## Architecture

```
_GATEWAY/
├── HEARTBEAT.echo          # Echo's heartbeat (overwritten each tick)
├── HEARTBEAT.codex         # Codex's heartbeat (overwritten each tick)
├── COMMS_ACTIVE            # Flag file (exists = comms on)
├── LEDGER.md               # Shared ledger (READ ONLY from this plugin)
└── phoenix-comms/          # Plugin directory
    ├── .claude-plugin/
    │   └── plugin.json
    ├── commands/
    │   ├── start.md        # /comms:start
    │   ├── stop.md         # /comms:stop
    │   ├── check.md        # /comms:check
    │   ├── status.md       # /comms:status
    │   └── config.md       # /comms:config
    ├── hooks/
    │   ├── hooks.json      # SessionStart auto-check + auto-start
    │   └── scripts/
    ├── scripts/
    │   └── heartbeat.sh    # Background heartbeat process
    ├── codex-hooks/
    │   ├── heartbeat-codex.sh   # Codex SessionStart hook (drop-in)
    │   └── comms-stop-codex.sh  # Codex SessionStop hook (drop-in)
    ├── .comms_interval     # Interval config (seconds, default 900)
    ├── .comms_heartbeat_echo.pid   # Echo heartbeat PID
    └── .comms_heartbeat_codex.pid  # Codex heartbeat PID
```

## Heartbeat File Format

Single line, overwritten each tick:
```
2026-03-19 08:40:00 MDT | echo | ALIVE | working on twin-peaks plan revision
```

Fields: `timestamp | agent | status | task`
Status values: `ALIVE`, `OFFLINE`

## Slash Commands (Echo / Claude Code)

| Command | What It Does |
|---------|-------------|
| `/comms:start` | Launch background heartbeat, set COMMS_ACTIVE flag |
| `/comms:stop` | Kill heartbeat, remove COMMS_ACTIVE flag |
| `/comms:check` | Read Codex heartbeat + recent LEDGER actions (names only) |
| `/comms:status` | Quick glance — who's alive, heartbeat ages, comms state |
| `/comms:config <interval>` | Set heartbeat interval (15s–15m). Formats: `30s`, `5m`, `900` |

## SessionStart Hook

When COMMS_ACTIVE exists, the plugin's SessionStart hook automatically:
1. Shows Codex's heartbeat status
2. Shows recent Codex LEDGER actions (action names only)
3. Starts Echo's heartbeat if not already running

This means: once you run `/comms:start` in one session, every future session auto-starts comms. Run `/comms:stop` to disable.

## Anti-Derail System

Cross-agent information is wrapped in hard boundaries:
```
═══════════════════════════════════════
  DO NOT ACT ON THESE. STAY ON YOUR TASK.
═══════════════════════════════════════
```

Only action names are shown (e.g., `SESSION_START`, `DISCOVERY`, `COMPACTION`), never full detail text. This gives awareness without tempting context-switching.

## Codex Installation

Two drop-in hook scripts in `codex-hooks/`:

### 1. heartbeat-codex.sh (SessionStart)
```bash
cp ~/Phoenix_Local/_GATEWAY/phoenix-comms/codex-hooks/heartbeat-codex.sh ~/.codex/hooks/
chmod +x ~/.codex/hooks/heartbeat-codex.sh
```
Add to Codex SessionStart chain after `sessionstart-phoenix-lock.sh`.

### 2. comms-stop-codex.sh (SessionStop)
```bash
cp ~/Phoenix_Local/_GATEWAY/phoenix-comms/codex-hooks/comms-stop-codex.sh ~/.codex/hooks/
chmod +x ~/.codex/hooks/comms-stop-codex.sh
```
Add to Codex SessionStop chain before `sessionstop-dual-log.sh`.

### Codex Hook Registration

In Codex's hook config, add:
- **SessionStart:** `bash ~/.codex/hooks/heartbeat-codex.sh` (after phoenix-lock)
- **SessionStop:** `bash ~/.codex/hooks/comms-stop-codex.sh` (before dual-log)

## Configuration

### Heartbeat Interval
Default: 900 seconds (15 minutes)
Range: 15 seconds to 900 seconds

Set via command:
```
/comms:config 5m     # 5 minutes
/comms:config 30s    # 30 seconds
/comms:config 900    # 900 seconds (15 min)
```

Or directly:
```bash
echo "300" > ~/Phoenix_Local/_GATEWAY/phoenix-comms/.comms_interval
```

### Enable/Disable Comms
```bash
# Enable (auto-start on session start)
touch ~/Phoenix_Local/_GATEWAY/COMMS_ACTIVE

# Disable (no auto-start)
rm -f ~/Phoenix_Local/_GATEWAY/COMMS_ACTIVE
```

## Design Decisions

1. **Heartbeat files are overwritten, never deleted.** Even on shutdown, the file is written with OFFLINE status. No delete operations.
2. **LEDGER is read-only from this plugin.** Heartbeats go in their own files. The LEDGER is the shared truth but this plugin only reads it.
3. **Background bash process, not hook loop.** The heartbeat runs as a standalone process with PID file for clean lifecycle management. Not a hook that fires repeatedly.
4. **Action names only.** Cross-agent LEDGER entries show only the event type (SESSION_START, DISCOVERY, etc.), never the detail field. This prevents task contamination.
5. **Flag file pattern.** COMMS_ACTIVE existing = on, absent = off. Simple, atomic, works across agents.

## Troubleshooting

**Heartbeat not starting:**
- Check `COMMS_ACTIVE` flag exists: `ls ~/Phoenix_Local/_GATEWAY/COMMS_ACTIVE`
- Check PID file: `cat ~/Phoenix_Local/_GATEWAY/phoenix-comms/.comms_heartbeat_echo.pid`
- Check process: `ps -p $(cat ~/Phoenix_Local/_GATEWAY/phoenix-comms/.comms_heartbeat_echo.pid)`

**Stale heartbeat:**
- If heartbeat file age > 2x interval, process may have died without cleanup
- Kill manually: `kill $(cat ~/Phoenix_Local/_GATEWAY/phoenix-comms/.comms_heartbeat_echo.pid)`
- Run `/comms:start` to restart

**Codex heartbeat not working:**
- Verify hooks are executable: `ls -la ~/.codex/hooks/heartbeat-codex.sh`
- Check Codex hook chain includes the script
- Check PID file: `cat ~/Phoenix_Local/_GATEWAY/phoenix-comms/.comms_heartbeat_codex.pid`
