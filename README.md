# Airweave for Claude
Official Airweave plugin for Claude Code and Cowork.

Airweave is a context retrieval layer between Claude and your apps like Notion, Slack, Google Drive, Linear, Jira, GitHub, and Salesforce. Search your apps directly from Claude Code and Cowork. Airweave continually syncs and indexes data from 40+ sources into unified searchable collections with agentic, semantic, and keyword search.

## Installation

### From Claude Code

```
claude plugin install airweave
```

### Manual installation

Clone and install the plugin:

```bash
git clone https://github.com/airweave-ai/claude-plugin
claude --plugin-dir ./claude-plugin
```

### Configuration

After installing, set your Airweave credentials:

```bash
export AIRWEAVE_API_KEY="your-api-key"
export AIRWEAVE_COLLECTION="your-collection-id"
```

Get your API key from your Airweave dashboard at:

- [app.airweave.ai](https://app.airweave.ai) for Airweave Cloud users
- or for local deployments, set base_url to `http://localhost:8001`

<video src="./assets/airweave_api_key.mp4" controls autoplay muted loop playsinline width="100%"></video>

Get your Airweave Collection ID from your collection view in the dashboard:

<video src="./assets/airweave_collection_id.mp4" controls autoplay muted loop playsinline width="100%"></video>

Or by listing your collections and taking the `readable_id` of your desired collection:

```typescript
import { AirweaveSDKClient } from "@airweave/sdk";

const client = new AirweaveSDKClient({ apiKey: "YOUR_AIRWEAVE_API_KEY" });
await client.collections.list({
    skip: 0,
    limit: 100
});

```

## Prerequisites

- [Airweave account](https://app.airweave.ai) (cloud) or [self-hosted instance](https://github.com/airweave-ai/airweave)
- At least one collection with connected data sources

## Features

This plugin provides an MCP server and two skills for working with Airweave:

- **Cross-app search** — Query Slack, GitHub, Notion, Jira, Google Drive, Salesforce, and 40+ other sources from a single interface
- **Hybrid search** — Combine agentic, neural, keyword, and hybrid search methods with query expansion, filtering, and reranking
- **Smart parameter tuning** — Enable Claude to do automatic selection of search strategy based on your query intent
- **AI completions** — Get synthesized answers with citations across all your connected data, not just raw results

## Skills

Skills are auto-invoked based on context:

| Skill | Triggers on |
|-------|-------------|
| `airweave-search` | "find messages about...", "search my docs for...", "what did the team discuss..." |
| `airweave-setup` | "set up Airweave", "connect Slack to Airweave", "install the Airweave SDK" |

### airweave-search

Helps Claude search and retrieve context from your connected data sources. Handles query formulation, parameter selection, result interpretation, and refinement when results are poor.

### airweave-setup

Guides developers through installing Airweave, creating collections, connecting data sources, and integrating the SDK (Python/TypeScript) or MCP server into their applications.

## Example Usage

```
> What did the team discuss about the product launch in Slack and does it align with the state of the project in Linear and Notion?
Claude searches your connected Slack, Linear, and Notion workspaces and synthesizes relevant messages.

> Find the API spec for our authentication service
Claude searches across GitHub repos, Confluence docs, and Jira tickets for implementation context.

> What's our current refund policy and when was it last updated?
Claude searches company wikis and Google Drive, Jira, and Stripe to find the authoritative document.

> Check Jira, Stripe, and Github for open bugs related to payments
Claude queries your Jira project and compiles a summary of outstanding issues.

> Search our Finance stack for the Q4 board deck
Claude locates files across shared drives and Salesforce using semantic search.
```

## Supported Sources

Airweave connects to [40+ data sources](https://docs.airweave.ai/connectors/overview) including:

| Category | Sources |
|----------|---------|
| Productivity & Collaboration | Asana, Airtable, ClickUp, Linear, Monday, Notion, Trello, Todoist |
| Cloud Storage & Documents | Box, Dropbox, Google Docs, Google Drive, Google Slides, OneDrive, OneNote, SharePoint, Word |
| Developer Tools | Confluence, Bitbucket, GitHub, GitLab, Jira |
| CRM & Sales | Attio, HubSpot, Pipedrive, Salesforce, Zoho CRM |
| Communication & Email | Gmail, Google Calendar, Outlook Calendar, Outlook Mail, Slack, Teams |
| Support & Service | Zendesk |
| Databases | PostgreSQL |
| E-commerce & Payments | Shopify, Stripe |

See the full list at [docs.airweave.ai/connectors/overview](https://docs.airweave.ai/connectors/overview).

## Documentation

- [Airweave docs](https://docs.airweave.ai)
- [Airweave Cloud](https://app.airweave.ai)
- [GitHub](https://github.com/airweave-ai/airweave)
- [Discord](https://discord.gg/484HY9Ehxt)

## License

MIT
