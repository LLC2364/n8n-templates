---
name: n8n
description: "Manage n8n workflows, trigger executions, and monitor runs. Use when: user wants to control n8n automations, trigger workflows, check execution status, enable/disable workflows, or manage n8n credentials. Requires n8n instance URL and auth token."
homepage: https://n8n.io
metadata:
  {
    "openclaw":
      {
        "emoji": "🔄",
        "requires": { "bins": ["curl", "jq"] },
        "primaryEnv": ["N8N_URL", "N8N_TOKEN"],
      },
  }
---

# n8n — Workflow Automation Manager

Connect to and manage your n8n instance via the REST API.

## Setup

Set environment variables:

```bash
export N8N_URL="https://n8n.example.com"      # Your n8n instance base URL
export N8N_TOKEN="your-n8n-api-key"            # API key from n8n settings
```

Or configure in `~/.openclaw/openclaw.json`:

```json
{
  "skills": {
    "entries": {
      "n8n": {
        "url": "https://n8n.example.com",
        "apiKey": "your-n8n-api-key"
      }
    }
  }
}
```

## Base URL

All API calls use: `${N8N_URL}/rest`

Auth header: `Authorization: Bearer ${N8N_TOKEN}`

## Quick Commands

### List Workflows

```bash
curl -s -H "Authorization: Bearer ${N8N_TOKEN}" \
  "${N8N_URL}/rest/workflows?limit=50&active=true"
```

### Get Workflow

```bash
curl -s -H "Authorization: Bearer ${N8N_TOKEN}" \
  "${N8N_URL}/rest/workflows/{id}"
```

### Trigger Workflow (Manual)

```bash
curl -s -X POST -H "Authorization: Bearer ${N8N_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{"data": {"key": "value"}}' \
  "${N8N_URL}/rest/workflows/{id}/execute"
```

### Get Executions

```bash
curl -s -H "Authorization: Bearer ${N8N_TOKEN}" \
  "${N8N_URL}/rest/executions?limit=20"
```

### Get Execution Details

```bash
curl -s -H "Authorization: Bearer ${N8N_TOKEN}" \
  "${N8N_URL}/rest/executions/{id}"
```

### Toggle Workflow Active/Inactive

```bash
# Activate
curl -s -X PATCH -H "Authorization: Bearer ${N8N_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{"active": true}' \
  "${N8N_URL}/rest/workflows/{id}"

# Deactivate
curl -s -X PATCH -H "Authorization: Bearer ${N8N_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{"active": false}' \
  "${N8N_URL}/rest/workflows/{id}"
```

### Delete Workflow

```bash
curl -s -X DELETE -H "Authorization: Bearer ${N8N_TOKEN}" \
  "${N8N_URL}/rest/workflows/{id}"
```

### List Credentials

```bash
curl -s -H "Authorization: Bearer ${N8N_TOKEN}" \
  "${N8N_URL}/rest/credentials"
```

### Get Credential

```bash
curl -s -H "Authorization: Bearer ${N8N_TOKEN}" \
  "${N8N_URL}/rest/credentials/{id}"
```

### Test Credential

```bash
curl -s -X POST -H "Authorization: Bearer ${N8N_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{"credentialId": "{id}"}' \
  "${N8N_URL}/rest/credentials/{id}/test"
```

## Common Tasks

### "List all active workflows"

```bash
curl -s -H "Authorization: Bearer ${N8N_TOKEN}" \
  "${N8N_URL}/rest/workflows?active=true" | jq '.data[] | "{.id}: {.name} (active)"'
```

### "Trigger a workflow with data"

```bash
curl -s -X POST -H "Authorization: Bearer ${N8N_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{"data": {"email": "user@example.com", "action": "signup"}}' \
  "${N8N_URL}/rest/workflows/{id}/execute"
```

### "Check recent failed executions"

```bash
curl -s -H "Authorization: Bearer ${N8N_TOKEN}" \
  "${N8N_URL}/rest/executions?limit=20&status=error" | jq '.data[] | "{.id}: {.workflowData.name} - {.stoppedAt}'"
```

### "Disable a workflow"

```bash
curl -s -X PATCH -H "Authorization: Bearer ${N8N_TOKEN}" \
  -H "Content-Type: application/json" \
  -d '{"active": false}' \
  "${N8N_URL}/rest/workflows/{id}" | jq '{id: .id, name: .name, active: .active}'
```

### "Get workflow stats"

```bash
# Success rate for last 50 runs
curl -s -H "Authorization: Bearer ${N8N_TOKEN}" \
  "${N8N_URL}/rest/executions?limit=50&workflowId={id}" | jq '{
    total: (.data | length),
    success: (.data | map(select(.status == "success")) | length),
    error: (.data | map(select(.status == "error")) | length)
  }'
```

## Notes

- n8n API base: `${N8N_URL}/rest`
- API key found in n8n Settings → API Key
- Self-hosted n8n uses Bearer token auth
- n8n Cloud uses API key from settings
- Rate limit: be respectful, add delays between bulk operations
- Execution data may be large — use `?limit=` to paginate
- Some n8n editions have limited API access (check your edition)

## Troubleshooting

### "Connection refused"

- Check N8N_URL is reachable from your machine
- Verify n8n is running: `curl ${N8N_URL}/health`
- Check if n8n needs HTTPS (recommended for production)

### "401 Unauthorized"

- Verify N8N_TOKEN is correct
- For n8n Cloud: use the full API key from settings
- For self-hosted: ensure `N8N_AUTH_LEVEL` is not blocking API access

### "404 Not Found"

- Some endpoints vary by n8n version — check your n8n version
- Community edition has limited API compared to Enterprise
