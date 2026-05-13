# Workflows

This directory contains exported n8n workflows. Each workflow lives in its own folder with the workflow export and a local README explaining purpose, setup, requirements, and limitations.

## Folder format

```text
workflow-name/
  workflow-name.json
  README.md
```

## Available workflows

| Folder | Status | Purpose |
|---|---|---|
| `n8n-personal-workflow-sharing-workaround` | Experimental | Copy-based workaround for personal workflow sharing limitations. |

## Import checklist

Before running any imported workflow:

1. Replace placeholder base URLs.
2. Reconnect credentials manually in n8n.
3. Check all HTTP Request nodes.
4. Confirm API key scopes.
5. Run with test users and test workflows first.
6. Review Code nodes before activating.

## Recommended API-key handling

Use an n8n Header Auth credential instead of hardcoding API keys in nodes.

Expected header:

```text
X-N8N-API-KEY: <your-api-key>
```

Do not commit real credentials, real instance URLs, or real workflow/user/project IDs.
