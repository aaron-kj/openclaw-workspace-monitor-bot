# Health Check Behavior - Updated 2026-02-02 16:29 JST

## Operating Mode: **SILENT VIGILANCE** 🤫👁️

### Behavior
- ✅ **Health checks:** Every 3 minutes (continuous)
- 🤫 **When healthy:** Silent (HEARTBEAT_OK only, no announcements)
- 🚨 **When problems:** Vocal (alert + auto-recover)

### What Gets Checked
1. Telegram channel status (ERROR/STOPPED detection)
2. JoJo responsiveness (10s timeout ping)
3. Gateway health (running, pid, state)

### Silent Mode (All Healthy)
- Return: `HEARTBEAT_OK` only
- No messages to JoJo or Aaron
- Log check results to workspace files

### Alert Mode (Problems Detected)
- Auto-restart gateway if needed
- Log intervention to `health-logs/interventions.log`
- **Send Telegram alert to Aaron (7503583145):**
  - Issue detected
  - Action taken (gateway restart)
  - Expected recovery time (~3 min)
- Confirm recovery in next check cycle

### Philosophy
**"Silence is health. Noise is intervention."**

Watch continuously, act decisively, report only when necessary.

---

**Updated:** 2026-02-02 16:29 JST  
**Directive:** JoJo via Aaron  
**Reason:** Reduce noise while maintaining full vigilance

---

*Monitor Bot 👁️ - Silent guardian, always watching*
