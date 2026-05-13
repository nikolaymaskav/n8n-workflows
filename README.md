# n8n Workflow Lab

A small collection of n8n workflow exports, experiments, and reusable automation patterns.

The workflows in this repository are intended as practical examples. They may need adjustment depending on your n8n version, hosting model, API scopes, user roles, and project/workspace setup.

## Repository structure

```text
Workflows/
  n8n-personal-workflow-sharing-workaround/
    n8n-personal-workflow-sharing-workaround.json
    README.md
```

## How to use a workflow

1. Open n8n.
2. Go to **Workflows**.
3. Import the relevant `.json` file.
4. Reconnect credentials after import.
5. Replace placeholder URLs such as `https://n8n.example.com` with your own n8n base URL.
6. Review every HTTP Request node before activating.
7. Test with non-production workflows first.

## Security notes

Published workflow exports should not contain:

- real API keys
- real credential IDs
- internal hostnames
- production webhook IDs
- real user emails
- company-specific names
- infrastructure-specific IDs

All workflows in this repository should use placeholders where environment-specific values are required.

## Current workflows

| Workflow | Status | Description |
|---|---|---|
| `n8n-personal-workflow-sharing-workaround` | Experimental | Validates workflow ownership, validates a target user, finds the target user's personal project, and creates a copy-based workaround when direct workflow sharing is not available through the public API. |

## Disclaimer

These workflows are provided as implementation examples. They are not drop-in production systems. Review permissions, API scopes, data exposure, and failure handling before using them in a real environment.
