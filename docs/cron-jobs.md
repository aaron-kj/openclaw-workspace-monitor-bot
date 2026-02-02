# Monitor Bot Cron Jobs

**Created:** 2026-02-02 15:14 JST  
**Status:** ✅ All jobs active and scheduled

---

## Active Jobs

### 1. Quick Health Check (every 3 min)
**ID:** `ac76f18e-cd41-4b32-a6bb-daad8ea349a0`  
**Schedule:** Every 180,000ms (3 minutes)  
**Session:** Isolated  
**Timeout:** 60 seconds

**Tasks:**
1. Check Telegram channel status via `openclaw status`
2. Ping JoJo via `sessions_send` (10s timeout)
3. If Telegram ERROR/STOPPED or JoJo unresponsive → auto-restart gateway
4. Log interventions to `health-logs/interventions.log`
5. Return `HEARTBEAT_OK` if all healthy

**Next run:** Every 3 minutes continuously

---

### 2. Daily Summary Report (8:00 AM JST)
**ID:** `36aa85f5-bb62-47ff-b657-fec7253b7760`  
**Schedule:** Daily at 8:00 AM (Asia/Tokyo)  
**Session:** Isolated  
**Timeout:** 120 seconds  
**Delivery:** Telegram to Aaron (7503583145)

**Tasks:**
1. Channel health stats (Telegram uptime, timeouts)
2. JoJo responsiveness metrics
3. Gateway uptime and restarts
4. System metrics (memory, disk, sessions)
5. Review `health-logs/interventions.log`
6. Security status
7. Recommendations
8. Save to `reports/daily-YYYY-MM-DD.md`
9. Deliver summary to Aaron via Telegram

**Next run:** Tomorrow 8:00 AM JST (and daily thereafter)

---

### 3. Weekly Security Audit (Monday 9:00 AM JST)
**ID:** `72364e41-2062-4be4-b7d6-51a34462b1b6`  
**Schedule:** Every Monday at 9:00 AM (Asia/Tokyo)  
**Session:** Isolated  
**Timeout:** 180 seconds  
**Delivery:** Telegram to Aaron (7503583145)

**Tasks:**
1. Execute `openclaw security audit --deep`
2. Save output to `reports/security-YYYY-MM-DD.txt`
3. Analyze CRITICAL/WARN/INFO issues
4. **If CRITICAL** → Immediate Telegram alert to Aaron
5. Generate weekly security report with:
   - Audit results
   - Security trends
   - Recommendations
   - Comparison to previous week
   - JoJo's security learning notes (10+ checks)
6. Save to `reports/weekly-security-YYYY-MM-DD.md`
7. Deliver summary to Aaron via Telegram

**Next run:** Next Monday 9:00 AM JST

---

## Job Management Commands

**List all jobs:**
```bash
openclaw cron list
```

**Check job status:**
```bash
openclaw cron runs --job-id <id>
```

**Trigger manually:**
```bash
openclaw cron run --job-id <id>
```

**Remove job:**
```bash
openclaw cron remove --job-id <id>
```

**Update job:**
```bash
openclaw cron update --job-id <id> --patch <changes.json>
```

---

## Integration with Other Systems

### Logging
- **Interventions:** `health-logs/interventions.log`
- **Daily reports:** `reports/daily-YYYY-MM-DD.md`
- **Security audits:** `reports/security-YYYY-MM-DD.txt`
- **Weekly reports:** `reports/weekly-security-YYYY-MM-DD.md`

### Communication
- Daily reports → Aaron via Telegram
- Security alerts → Immediate Telegram notification
- Routine checks → Silent (HEARTBEAT_OK) unless issues

### Emergency Response
All cron jobs can:
- Auto-restart gateway when needed
- Log interventions for audit trail
- Escalate to Aaron for critical issues

---

## Monitoring the Monitors

To verify cron jobs are running:
```bash
# Check last run times
openclaw cron list | grep monitor-bot

# View recent job execution logs
openclaw logs | grep cron
```

**Expected behavior:**
- Health check runs every 3 minutes
- Daily report at 8:00 AM JST every day
- Security audit at 9:00 AM JST every Monday

---

*Monitor Bot 👁️ - Always watching, always ready*
