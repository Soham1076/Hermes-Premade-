# 📝 Skill: multi-platform-gateway

> **Category**: autonomous-ai-agents • **Version**: 1.0.0 • **Status**: Available

---

## 📋 Overview
Fix silent Hermes gateway bots on Discord, Slack, Telegram.

---

## 🎯 Use When
*[Auto-extracted from skill - customize based on actual usage patterns]*

---

## 📖 Full Skill Documentation

## When to Use

**Trigger:** Hermes gateway shows platform as `✓ connected` but bot doesn't respond to messages — no replies, no errors visible to user, messages appear to send but are silently dropped.

**Applies when:** You've configured Discord, Slack, or Telegram bot tokens and the gateway connects successfully, but the bot ignores your messages. Common after initial setup or when adding new users.

---

# Multi-Platform Gateway Troubleshooting

Hermes runs a single agent across multiple messaging platforms via the **gateway**. Each platform has its own authentication, allowlist, and permission model. This skill covers the most common "connected but not responding" issues.

## Quick Diagnosis

| Symptom | Likely Cause |
|---------|--------------|
| Gateway shows `✓ connected` but no replies | Allowlist not configured (user not authorized) |
| Discord: "Unauthorized user" in logs | Missing `DISCORD_ALLOWED_USERS` or `DISCORD_ALLOW_ALL_USERS` |
| Slack: messages send but bot ignores | Missing `SLACK_ALLOWED_USERS` or scopes not reinstalled |
| Telegram: works in DM but not groups | Group privacy mode or missing `TELEGRAM_ALLOWED_USERS` |
| Bot responds on one platform but not another | Each platform has independent allowlist config |

---

## Discord

### Required Bot Setup
1. **Developer Portal** → Application → Bot tab
2. **Enable Privileged Gateway Intents:**
   - Message Content Intent ✓
   - Server Members Intent ✓
3. **Copy Token** → Add to `.env` as `DISCORD_BOT_TOKEN`

### Allowlist Configuration
```bash
# Allow all users (easiest for testing)
hermes config set env.DISCORD_ALLOW_ALL_USERS true

# Allow specific users (comma-separated Discord user IDs - 18 digit snowflakes)
hermes config set env.DISCORD_ALLOWED_USERS "783610078720163880,123456789012345678"

# Allow by role ID
hermes config set env.DISCORD_ALLOWED_ROLES "123456789012345678"

# Allow by channel ID
hermes config set env.DISCORD_ALLOWED_CHANNELS "123456789012345678,987654321098765432"

hermes gateway restart
```

### Common Errors
| Log Message | Fix |
|-------------|-----|
| `Unauthorized user: 783610078720163880` | Add user to `DISCORD_ALLOWED_USERS` |
| `401 Unauthorized: Improper token` | Regenerate bot token in Developer Portal |
| `Slash command sync failed: 401` | Bot token invalid or missing intents |

---

## Slack

### Required App Configuration
**OAuth & Permissions → Bot Token Scopes:**
```
app_mentions:read, channels:history, channels:read, chat:write, commands,
files:read, files:write, groups:history, groups:read, im:history, im:read,
im:write, mpim:history, mpim:read, reactions:read, users:read
```

**Event Subscriptions (Socket Mode):**
```
app_mention, message.channels, message.groups, message.im, message.mpim,
reaction_added, reaction_removed
```

**After scope changes:** Reinstall app at https://api.slack.com/apps → your app → Install App → **Reinstall to Workspace**

### Allowlist Configuration
```bash
# Allow all users
hermes config set env.SLACK_ALLOW_ALL_USERS true

# Allow specific users (Slack user IDs starting with U...)
hermes config set env.SLACK_ALLOWED_USERS "U0BPV2HN540,U1234567890"

hermes gateway restart
```

### Critical: Invite Bot to Conversations
Slack bot **must be invited** to each channel/DM:
- Channel: `/invite @Hermes`
- DM: Open DM with @Hermes directly

### Common Errors
| Log Message | Fix |
|-------------|-----|
| `missing 'mpim:history' scope` | Add scope + reinstall app |
| `Socket Mode connected` but no replies | User not in `SLACK_ALLOWED_USERS` |
| `message.im` not received | Bot not in DM, or missing `im:history` scope |

### Session Fix: Slack Allowlist User ID Mismatch (2026-08-09)
**Problem:** Slack connected (`✓ slack connected`) but messages silently dropped — no errors in gateway.log, no replies.

**Root Cause:** `.env` had `SLACK_ALLOWED_USERS=U0BPV2HN540` but that was **not the actual Slack user ID** of the person messaging.

**Fix:**
```bash
# Option 1: Allow all users (quick test)
hermes config set env.SLACK_ALLOW_ALL_USERS true
hermes gateway restart

# Option 2: Find actual Slack user ID
# In Slack: Right-click your name → "Copy member ID" (starts with U...)
# Then update:
hermes config set env.SLACK_ALLOWED_USERS "U0BPV2HN540,YOUR_ACTUAL_USER_ID"
hermes gateway restart

# Option 3: Clear allowlist entirely
hermes config unset env.SLACK_ALLOWED_USERS
hermes gateway restart
```

**Diagnostic:** Check `agent.log` (not gateway.log) for Slack auth:
```bash
grep -i "slack.*unauthorized\|slack.*not allowed\|slack.*allowed_users" ~/.hermes/logs/agent.log
```

---

## Telegram

### Setup
1. Talk to @BotFather → `/newbot` → copy token
2. Add to `.env` as `TELEGRAM_BOT_TOKEN`
3. `/setprivacy` → **Disable** (allows bot to see all group messages)

### Allowlist Configuration
```bash
# Allow all users
hermes config set env.TELEGRAM_ALLOW_ALL_USERS true

# Allow specific users (numeric Telegram user IDs)
hermes config set env.TELEGRAM_ALLOWED_USERS "1890648613,123456789"

hermes gateway restart
```

---

## Diagnostic Commands

```bash
# Gateway status
hermes gateway status

# Check allowlist config in active profile
grep -i "ALLOWED_USERS\|ALLOW_ALL" ~/.hermes/.env

# Gateway logs - auth errors
grep -i "unauthorized\|not allowed\|allowed_users" ~/.hermes/logs/gateway.log | tail -20

# Agent logs - platform issues
grep -i "slack\|discord\|telegram" ~/.hermes/logs/agent.log | tail -30

# Full gateway log tail
tail -100 ~/.hermes/logs/gateway.log
```

---

## Environment Variable Reference

| Platform | Allow All | Allow Users | Allow Roles | Allow Channels | Bot Token |
|----------|-----------|-------------|-------------|----------------|-----------|
| Discord | `DISCORD_ALLOW_ALL_USERS=true` | `DISCORD_ALLOWED_USERS` | `DISCORD_ALLOWED_ROLES` | `DISCORD_ALLOWED_CHANNELS` | `DISCORD_BOT_TOKEN` |
| Slack | `SLACK_ALLOW_ALL_USERS=true` | `SLACK_ALLOWED_USERS` | — | — | `SLACK_BOT_TOKEN`, `SLACK_APP_TOKEN` |
| Telegram | `TELEGRAM_ALLOW_ALL_USERS=true` | `TELEGRAM_ALLOWED_USERS` | — | — | `TELEGRAM_BOT_TOKEN` |

---

## Pitfalls Checklist

- [ ] **Restart gateway** after any `.env` change: `hermes gateway restart`
- [ ] **User ID format**: Discord = 18-digit snowflake, Slack = `U...`, Telegram = numeric
- [ ] **Slack scope changes** require **reinstall** in workspace
- [ ] **Slack bot must be invited** to each channel/DM (`/invite @Hermes`)
- [ ] **Discord intents**: Message Content + Server Members must be enabled
- [ ] **Telegram privacy**: `/setprivacy` → Disable for group messages
- [ ] **Check correct profile's `.env`**: Each profile has isolated config
- [ ] **Silent Slack drops**: Check `agent.log`, not just `gateway.log`
- [ ] **Multiple allowlist vars**: Only ONE needs to match (user OR role OR channel)

---

## 🔧 Tools & Commands Required
*Check skill's `required_commands` and `required_environment_variables`*

---

## 🔗 Related Skills
*[Add links to related skills in same category]*

---

## 📂 Skill Directory
`[HERMES_HOME]\skills\autonomous-ai-agents\multi-platform-gateway\`

---

*Source: `skill_view('multi-platform-gateway')`*
*Updated: 2026-08-10*
tags: [skill, autonomous-ai-agents, #skill/autonomous-ai-agents]
parent: "[[Ops/Skills-Registry/autonomous-ai-agents/MOC-Autonomousaiagents]]"
registry: "[[Ops/Skills-Registry/MOC-Skills-Registry]]"
catalog: "[[Ops/Skills-Registry/Catalog]]"
