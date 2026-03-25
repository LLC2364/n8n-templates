# n8n Templates for OpenClaw

Collection of n8n workflow templates + OpenClaw skill for managing your n8n instance via natural language.

## Files

- **`SKILL.md`** — OpenClaw skill: workflow management, executions, credentials, triggers
- **`templates/`** — Import into n8n directly
  - `schedule-trigger.json` — Cron → HTTP → Log
  - `webhook-formatter.json` — Webhook → Transform → Discord
  - `rss-monitor.json` — RSS → Filter new → Telegram
  - `database-health.json` — Schedule → DB query → Alert

## Setup

Configure your n8n instance:

```bash
export N8N_URL="https://n8n.example.com"
export N8N_TOKEN="your-api-key"
```

Or in `~/.openclaw/openclaw.json`:

```json
{
  "skills": {
    "entries": {
      "n8n": {
        "url": "https://n8n.example.com",
        "apiKey": "your-api-key"
      }
    }
  }
}
```

## Usage

Import templates: n8n → Workflows → Import from JSON → pick a file from `templates/`

Configure credentials in each template (Telegram chat ID, Discord webhook, DB connection).

## OpenClaw Commands

After setting up N8N_URL/N8N_TOKEN:
- "List all workflows"
- "Trigger workflow #42 with {data}"
- "Check failed executions"
- "Disable the RSS monitor workflow"
