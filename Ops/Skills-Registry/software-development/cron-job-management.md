# 📝 Skill: cron-job-management

> **Category**: software-development • **Version**: 1.0.0 • **Status**: Available

---

## 📋 Overview
Use when managing Hermes cron jobs: schedules and failures.

---

## 🎯 Use When
*[Auto-extracted from skill - customize based on actual usage patterns]*

---

## 📖 Full Skill Documentation

# Cron Job Management

Comprehensive guide for creating, debugging, and optimizing Hermes cron jobs.

## Quick Reference

### Schedule Syntax
- `*/15 * * * *` = every 15 minutes at :00, :15, :30, :45
- `0 9 * * *` = daily at 9 AM
- `30m` / `every 2h` = duration-based
- ISO timestamp = one-shot

### Key Commands
```bash
hermes cron create --name "job-name" --schedule "*/15 * * * *" --prompt "Task"
hermes cron list
hermes cron run <job_id>
hermes cron pause <job_id>
hermes cron resume <job_id>
hermes cron remove <job_id>
```

## Execution Model
- Background via gateway's internal scheduler
- Isolated agent session with `skip_memory=true`
- 3-minute hard interrupt per run
- `.tick.lock` prevents duplicate ticks

## Common Failures & Fixes

| Failure | Cause | Fix |
|---------|-------|-----|
| `ResourceExhausted: Worker local total request limit reached (32/32)` | NVIDIA provider concurrent cap | Add fallback_model; use local Ollama; space jobs >5 min |
| `last_status: error`, no delivery | Provider error | Check `agent.log` for `cron_<job_id>` |
| `executed: false`, `execution_skipped` | Job already running | Wait for completion |
| Job never fires | Gateway/scheduler down | `hermes gateway status`, restart |

## Monitoring
```bash
grep "cron_<job_id>" ~/.hermes/profiles/<profile>/logs/agent.log
grep "cron.scheduler" ~/.hermes/profiles/<profile>/logs/gateway.log
hermes cron list
```

## Delivery Options
| Value | Behavior |
|-------|----------|
| `"origin"` | Originating chat/platform |
| `"telegram"` | Force Telegram |
| `"local"` | File only |
| `"all"` | All home channels |

## Rate Limit Mitigation (Critical for Frequent Jobs)

1. **Add fallback_model** in config.yaml:
   ```yaml
   fallback_model:
     provider: openrouter
     model: anthropic/claude-sonnet-4
   ```

2. **Use local Ollama** for cron jobs:
   ```bash
   cronjob(action='create', model='ollama/llama3.1:8b', provider='ollama', ...)
   ```

3. **Space jobs >5 minutes apart**

## Example: Creative Heartbeat (15-min)

```bash
hermes cron create \
  --name "creative-heartbeat-15min" \
  --schedule "*/15 * * * *" \
  --prompt "Send creative Neko-chan message to Telegram chat 1890648613. Varied: haiku, cat fact, tiny story, ASCII art, whisper, fortune cookie, mini roleplay. <200 chars. Warm, possessive. No fake errors." \
  --deliver "origin"
```

## Session Notes (2026-08-09)

- **Job ID:** `3cd08e895c29`, **Schedule:** `*/15 * * * *`
- **Target:** Telegram chat `1890648613`
- **Gateway:** PID 984, **Model:** NVIDIA nemotron-3-ultra
- **Results:** 4:15 PM ✅, 4:30 PM ❌ (rate limit), 4:45 PM ⏳
- **Lesson:** NVIDIA 32-concurrent limit needs fallback/local model for 15-min cron

### Rate Limit Debugging Pattern (Session: 2026-08-09)
**Symptom:** Cron job shows `last_status: error` with `ResourceExhausted: Worker local total request limit reached (33/32)`

**Diagnosis:**
```bash
# Check agent log for cron execution
grep "cron_3cd08e895c29" ~/.hermes/profiles/[PROFILE]/logs/agent.log

# Check gateway scheduler log
grep "cron.scheduler.*3cd08e895c29" ~/.hermes/profiles/[PROFILE]/logs/gateway.log
```

**Root Cause:** NVIDIA provider has 32 concurrent request cap. Multiple active sessions (Telegram, Discord, cron) share the pool.

**Fix Options (in order of preference):**
1. **Add fallback_model in config.yaml** - auto-failover on 429/503
2. **Use local Ollama model** for cron jobs: `--model ollama/llama3.1:8b --provider ollama`
3. **Space cron jobs >5 minutes apart** from other traffic
4. **Reduce cron frequency** to `*/30 * * * *` or longer

**Verification:** Next run should show `completed successfully` in `cron.scheduler` log and delivery to Telegram.

---

## 🔧 Tools & Commands Required
*Check skill's `required_commands` and `required_environment_variables`*

---

## 🔗 Related Skills
*[Add links to related skills in same category]*

---

## 📂 Skill Directory
`[HERMES_HOME]\skills\software-development\cron-job-management\`

---

*Source: `skill_view('cron-job-management')`*
*Updated: 2026-08-10*
tags: [skill, software-development, #skill/software-development]
parent: "[[Ops/Skills-Registry/software-development/MOC-Softwaredevelopment]]"
registry: "[[Ops/Skills-Registry/MOC-Skills-Registry]]"
catalog: "[[Ops/Skills-Registry/Catalog]]"
