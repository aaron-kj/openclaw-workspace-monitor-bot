# Security Checklist - 14-Point Comprehensive

**Purpose:** Weekly security audit reference  
**Source:** JoJo's security knowledge transfer (2026-02-02)  
**Frequency:** Every Monday 9:00 AM JST

---

## Quick Reference

| # | Check | Expected | Command/Location |
|---|-------|----------|------------------|
| 1 | OpenClaw audit | 0 CRITICAL | `openclaw security audit --deep` |
| 2 | Telegram groupPolicy | `"allowlist"` | `.channels.telegram.groupPolicy` |
| 3 | Telegram requireMention | `true` | `.channels.telegram.groups.*.requireMention` |
| 4 | Telegram groupAllowFrom | `["7503583145"]` | `.channels.telegram.groupAllowFrom` |
| 5 | Gateway bind | `"loopback"` | `.gateway.bind` |
| 6 | Gateway auth | `"token"` | `.gateway.auth.mode` |
| 7 | Primary model | Claude 4.5+ / GPT-5+ | `.agents.defaults.model.primary` |
| 8 | Elevated tools | Monitored in groups | Logs + allowlist |
| 9 | External services | Review new integrations | Skills, plugins |
| 10 | Firewall/ports | 18789 not external | `lsof -i :18789` |
| 11 | Secrets in logs | None leaked | `openclaw logs | grep secret` |
| 12 | Git repo access | No unauthorized commits | `git log` |
| 13 | Session hijacking | No unexpected sessions | `openclaw sessions list` |
| 14 | Config drift | No unauthorized changes | Config last modified time |

---

## Detailed Procedures

### 1. OpenClaw Security Audit
```bash
openclaw security audit --deep > reports/security-$(date +%Y-%m-%d).txt
grep "CRITICAL" reports/security-$(date +%Y-%m-%d).txt
# Expected: 0 matches
```

**Alert if:** CRITICAL > 0  
**Action:** Immediate Telegram alert to Aaron

---

### 2. Telegram groupPolicy
```bash
openclaw gateway config.get | jq '.config.channels.telegram.groupPolicy'
# Expected: "allowlist"
```

**Alert if:** Value is "open" or missing  
**Risk:** Unrestricted group access → prompt injection

---

### 3. Telegram requireMention
```bash
openclaw gateway config.get | jq '.config.channels.telegram.groups'
# Check each group's requireMention field
```

**Alert if:** Any group has `requireMention: false` without approval  
**Risk:** Bot processes every message → resource waste + privacy risk

---

### 4. Telegram groupAllowFrom
```bash
openclaw gateway config.get | jq '.config.channels.telegram.groupAllowFrom'
# Expected: ["7503583145"]
```

**Alert if:** Unknown user IDs added  
**Risk:** Unauthorized users can trigger bot in groups

---

### 5. Gateway Network Binding
```bash
openclaw status | grep "Gateway"
# Expected: ws://127.0.0.1:18789
```

**Alert if:** Binds to 0.0.0.0 or public IP  
**Risk:** CRITICAL - Gateway exposed to network

---

### 6. Gateway Auth Token
```bash
openclaw gateway config.get | jq '.config.gateway.auth.mode'
# Expected: "token"
```

**Alert if:** Mode is "none"  
**Risk:** CRITICAL - Unauthorized access possible

---

### 7. Model Security
```bash
openclaw gateway config.get | jq '.config.agents.defaults.model.primary'
# Expected: "github-copilot/claude-sonnet-4.5" or GPT-5+
```

**Alert if:** Primary model downgrades to Haiku or older models  
**Risk:** Increased prompt injection vulnerability

---

### 8. Elevated Tools in Groups
```bash
openclaw logs --limit 200 | grep -E "telegram.*group.*(exec|write|gateway restart)"
```

**Alert if:** Elevated tools used by non-Aaron users  
**Risk:** Prompt injection → dangerous commands

---

### 9. External Service Integrations
```bash
# Check recently installed skills
ls -lt ~/.openclaw/workspace/skills/ | head -10

# Check plugins
openclaw gateway config.get | jq '.config.plugins.entries'
```

**Alert if:** New services added without security review  
**Risk:** Malicious or vulnerable integrations

---

### 10. Firewall & Port Security
```bash
lsof -i :18789 2>/dev/null
# Expected: localhost:18789 only (no external bindings)
```

**Alert if:** Port accessible from external network  
**Risk:** CRITICAL - Gateway exposed

---

### 11. Secrets in Logs
```bash
openclaw logs --limit 500 | grep -iE "(password|token|api[_-]?key|secret)" | grep -v "auth token" | grep -v "bot token"
```

**Alert if:** Secrets found in command output  
**Risk:** Credential leakage

---

### 12. Git Repository Access
```bash
cd ~/.openclaw/workspace-monitor-bot
git log --oneline --since="7 days ago"
# Check for unexpected commits
```

**Alert if:** Unknown committers or suspicious changes  
**Risk:** Unauthorized access or tampering

---

### 13. Session Hijacking Detection
```bash
openclaw sessions list --json | jq '.[] | {key, kind, channel}'
```

**Alert if:** Unknown sessions or abnormal count (>30)  
**Risk:** Unauthorized access or session leak

---

### 14. Configuration Drift
```bash
stat -f "%Sm" ~/.openclaw/openclaw.json
# Check last modification time
```

**Alert if:** Config modified without known operation  
**Risk:** Unauthorized configuration changes

---

## Weekly Audit Workflow

1. Run all 14 checks systematically
2. Document findings in `reports/security-weekly-YYYY-MM-DD.md`
3. Count issues:
   - CRITICAL → Immediate alert
   - WARN → Document and track
   - INFO → Note for awareness
4. Compare to previous week's report
5. Generate summary and recommendations
6. Deliver report to Aaron via Telegram (if issues) or include in daily summary (if all clear)

---

## Alert Thresholds

| Severity | Action | Timing |
|----------|--------|--------|
| CRITICAL | Immediate Telegram alert | Within 1 minute |
| WARN (new) | Include in weekly report | Monday 9 AM |
| WARN (persistent) | Escalate after 4 weeks | In weekly report |
| INFO | Document only | In weekly report |

---

## Security Incident Response

### If CRITICAL Issue Found:

**DO:**
1. Alert Aaron immediately via Telegram
2. Include: Issue, risk, affected component, recommended fix
3. Log in `security-incidents.log`
4. Create incident report in `reports/security-incidents/`

**DON'T:**
1. Fix automatically (needs human approval)
2. Downplay severity
3. Wait for next report
4. Assume Aaron knows

---

**Last Updated:** 2026-02-02 15:22 JST  
**Version:** 1.0  
**Next Review:** After first production security audit

---

*Monitor Bot 👁️ - Keeping the ecosystem secure*
