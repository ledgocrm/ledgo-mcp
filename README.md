# Ledgo CRM MCP server

Connect an AI assistant to [Ledgo CRM](https://ledgo.io/) through the Model Context Protocol (MCP). The assistant can find customers, read a lead's history, add leads, plan follow-ups, move deals between stages, log calls and create tasks, always with the permissions of the person whose API key it uses.

Ledgo CRM is a CRM for teams that sell over WhatsApp and the phone: one shared inbox, leads with follow-ups, quotes and invoices. This repository documents its hosted MCP server; there is nothing to install or run.

| | |
|---|---|
| Endpoint | `https://ledgo.io/api/mcp` |
| Transport | Streamable HTTP, stateless JSON-RPC 2.0 |
| Protocol versions | 2025-06-18, 2025-03-26, 2024-11-05 |
| Authentication | `Authorization: Bearer agk_…` (a Ledgo API key) |
| Rate limit | 120 requests a minute per workspace |
| Plans | API keys are available on the Business plan and the 14-day trial |

## Get an API key

1. Start a workspace at [ledgo.io](https://ledgo.io/) (14-day trial, no card).
2. Open **Settings → Integrations** and create an API key.
3. The key acts as you: a sales rep's key sees only that rep's leads, and every change is recorded in the workspace's History under the key's name.

## Connect a client

**Claude Code**

```bash
claude mcp add --transport http ledgo https://ledgo.io/api/mcp --header "Authorization: Bearer agk_your_key"
```

**Claude Desktop, Cursor, Windsurf and other clients that take a JSON config**

```json
{
  "mcpServers": {
    "ledgo": {
      "url": "https://ledgo.io/api/mcp",
      "headers": { "Authorization": "Bearer agk_your_key" }
    }
  }
}
```

Clients that only start local servers can bridge with [mcp-remote](https://www.npmjs.com/package/mcp-remote):

```json
{
  "mcpServers": {
    "ledgo": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://ledgo.io/api/mcp", "--header", "Authorization: Bearer agk_your_key"]
    }
  }
}
```

## Tools

| Tool | What it does |
|---|---|
| `whoami` | The workspace, your role, currency, time zone and today's date. Call this first. |
| `search` | Find leads, deals, contacts and companies by name, company, phone or email. |
| `list_pipelines` | Pipelines and stage names for leads or deals. |
| `list_records` | Leads or deals filtered by status, owner, follow-up (overdue, today, none, stale), stage or text. |
| `get_record` | One lead, deal, contact or company with phones, emails, recent timeline and open tasks. |
| `create_lead` | Add a lead; the contact is matched by phone or email first, so repeat customers are not duplicated. |
| `set_follow_up` | Plan or clear the next follow-up of a lead or deal. |
| `move_stage` | Move a lead or deal to another stage by name; lost stages need a reason. |
| `log_activity` | Log a note, call, meeting or visit, optionally planning the next follow-up. |
| `my_day` | Overdue follow-ups, today's list, untouched new leads, missed calls and unanswered messages. |
| `list_tasks` | Tasks by overdue, today, upcoming, someday and done; yours or the team's. |
| `create_task` | Create a task, optionally linked to a record and assigned to a team member. |
| `team` | The workspace's members, for assigning tasks and leads. |
| `report` | Sales figures for a period: new leads, won deals, values and conversion. |

Dates are ISO 8601 with an offset, for example `2026-10-21T10:00:00+04:00`.

## Example prompts

- "What's on my list today in Ledgo?"
- "Add a lead for Sara Ahmed, +971 50 123 4567, interested in a kitchen fit-out, follow up tomorrow at 10."
- "Move the Al Noor deal to Quotation sent and remind me on Thursday."
- "Which leads have had no follow-up for two weeks?"

## Also available

- REST API and signed webhooks: [ledgo.io/developers](https://ledgo.io/developers) (OpenAPI 3.1 at `https://ledgo.io/api/v1/openapi.json`)
- Product overview for assistants: [ledgo.io/llms.txt](https://ledgo.io/llms.txt)
- Feature page: [CRM REST API, webhooks and AI connection](https://ledgo.io/features/api-and-webhooks)

## Security

Every call runs through the same services and permission checks as the Ledgo app, scoped to one workspace. Revoke a key at any time in Settings → Integrations. Report security issues to support@ledgo.io.

## License

This documentation is available under the MIT License. Ledgo CRM itself is a hosted commercial service; see [ledgo.io/terms](https://ledgo.io/terms).
