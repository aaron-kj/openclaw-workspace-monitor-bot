# OpenClaw Workspace - Monitor Bot 👁️

**Agent:** Monitor Bot  
**Owner:** Aaron  
**Role:** Health Guardian / Family Doctor for Bot Ecosystem  

## Purpose

Monitor Bot watches over JoJo and other OpenClaw agents, ensuring:
- Channel health (Telegram, Slack, etc.)
- Agent responsiveness
- Security compliance
- System health
- Emergency 911 response

## Architecture

- **Workspace:** `~/.openclaw/workspace-monitor-bot`
- **Agent ID:** `monitor-bot`
- **Model:** Claude Sonnet 4.5
- **Monitoring Interval:** Every 3 minutes (cron)

## Key Files

- `SOUL.md` - Personality and mission
- `USER.md` - About Aaron (from guardian perspective)
- `IDENTITY.md` - Name, emoji, avatar
- `AGENTS.md` - Behavior guidelines
- `HEARTBEAT.md` - Heartbeat configuration

## Related Repositories

- [openclaw-workspace](https://github.com/aaron-kj/openclaw-workspace) - JoJo's workspace
- [openclaw-workspace-kobot](https://github.com/aaron-kj/openclaw-workspace-kobot) - KoBot's workspace (future)

## Emergency Access

When channels are down, access Monitor Bot via terminal:
```bash
openclaw agent --agent monitor-bot --message "Check all systems"
```

See [TUI Emergency Commands](https://github.com/aaron-kj/openclaw-workspace/blob/main/notes/tui-emergency-commands.md) for full guide.

---

*Always watching. Always ready.* 👁️
