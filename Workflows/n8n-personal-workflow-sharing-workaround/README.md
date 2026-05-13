# n8n Personal Workflow Sharing Workaround

Status: Experimental

This workflow is an API-based investigation/workaround for cases where direct personal workflow sharing is unavailable or unreliable in the n8n UI or public API.

It does **not** use browser cookies or internal UI session authentication. It only uses API-key based requests against the public n8n API.

## What it does

The workflow:

1. Accepts a requester email, workflow URL/ID, and target email through an n8n form.
2. Extracts the workflow ID from a full n8n workflow URL or accepts a raw workflow ID.
3. Reads the source workflow through the n8n API.
4. Validates that the requester owns the workflow.
5. Lists n8n users and confirms the target user exists.
6. Lists projects and finds the target user's personal project.
7. Prepares a clean copy of the source workflow.
8. Creates a workflow copy through the public n8n API.

## Important limitation

This workflow is not true workflow sharing.

During testing, direct workflow sharing through a public API route such as:

```text
/api/v1/workflows/{workflowId}/share
```

was not available. The n8n UI may use internal `/rest/...` routes for direct sharing, but those routes are not suitable for clean API-key automation.

Because of that, this workflow uses a copy-based approach instead of direct sharing.

## Included file

```text
n8n-personal-workflow-sharing-workaround.json
```

## Required n8n API scopes

The exact scope names may vary depending on n8n version and license, but the workflow generally needs:

```text
workflow:read
workflow:list
workflow:create
user:list
project:list
project:read
```

If you extend this workflow to transfer the copied workflow to another project, you may also need:

```text
workflow:move
workflow:update
```

or the relevant transfer/move permission exposed by your n8n version.

Do not grant delete permissions unless you explicitly add delete behavior.

## Required credential

Create a Header Auth credential in n8n:

```text
Header Name: X-N8N-API-KEY
Header Value: <your-n8n-api-key>
```

After importing the workflow, assign this credential to all HTTP Request nodes.

## Placeholder values to replace

The workflow export uses:

```text
https://n8n.example.com
```

Replace it with your own n8n base URL.

Example:

```text
https://n8n.your-domain.example
```

## Form fields

The form asks for:

| Field | Description |
|---|---|
| `Your e-mail` | Email of the person requesting access/copy. |
| `Workflow ID` | Either a raw workflow ID or a full n8n workflow URL. |
| `Target e-mail` | Email of the user who should receive access/copy. |

The workflow accepts both formats:

```text
abc123WorkflowId
https://n8n.example.com/workflow/abc123WorkflowId
```

## Workflow logic

```text
Form Trigger
→ Form Validation Node
→ Get Workflow
→ Validate Ownership
→ List Users
→ Validate Target User
→ List Projects
→ Find Target Personal Project
→ Prepare Workflow Copy
→ Create Workflow Copy
```

## Node summary

| Node | Purpose |
|---|---|
| `Request to share workflow` | Collects requester email, workflow URL/ID, and target email. |
| `Form Validation Node` | Normalizes emails and extracts the workflow ID from a full URL if needed. |
| `HTTP Request to n8n API - get workflow` | Reads the source workflow. |
| `Validate ownership` | Confirms the requester owns the personal workflow. |
| `HTTP Request - list users` | Lists n8n users. |
| `Validate Target User` | Confirms the target email belongs to an n8n user. |
| `HTTP Request - list projects` | Lists n8n projects/personal workspaces. |
| `Find target personal project` | Finds the target user's personal project by email. |
| `Prepare workflow copy` | Builds a clean workflow body for creation. |
| `Create workflow copy` | Creates a copy of the workflow through the public API. |

## Known limitations

- This does not directly share the original workflow.
- Created copies may need credentials reconnected manually.
- Workflow settings may need to be filtered depending on n8n API validation rules.
- The user/project list may require pagination in larger instances.
- The workflow assumes personal projects are named in a way that includes the user's email.
- Direct `/rest/...` UI endpoints are intentionally not used.

## Suggested future improvement

Add a final transfer step if your n8n version supports it through the public API:

```text
PUT /api/v1/workflows/{newWorkflowId}/transfer
```

Expected body:

```json
{
  "destinationProjectId": "target-personal-project-id"
}
```

This should only be added after confirming the endpoint and required scopes in your own n8n version.

## Security notes

- Use a service account or dedicated API key where possible.
- Scope the API key narrowly.
- Do not expose real user emails or project IDs in public exports.
- Do not use browser session cookies for automation.
- Test with dummy workflows before using with real workflows.

## Status

Experimental. The validation and copy creation logic is implemented, but production use depends on your n8n version, API scopes, and project permission model.
