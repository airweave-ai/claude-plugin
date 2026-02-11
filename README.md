# Airweave for Claude

Search your apps, databases, and APIs directly from Claude. Airweave syncs and indexes data from 40+ sources into unified searchable collections with hybrid, neural, and keyword search.

## Installation

### From Claude Code

```
claude plugin install airweave
```

### Manual installation

Clone and install the plugin:

```bash
git clone https://github.com/airweave-ai/claude-plugin
claude plugin install --path ./claude-plugin
```

### Configuration

After installing, set your Airweave credentials:

```bash
export AIRWEAVE_API_KEY="your-api-key"
export AIRWEAVE_COLLECTION="your-collection-id"
```

Get your API key from [app.airweave.ai](https://app.airweave.ai) under Settings > API Keys.

## Prerequisites

- [Airweave account](https://app.airweave.ai) (cloud) or [self-hosted instance](https://github.com/airweave-ai/airweave)
- At least one collection with connected data sources

## Features

This plugin provides an MCP server and two skills for working with Airweave:

- **Cross-app search** — Query Slack, GitHub, Notion, Jira, Google Drive, Salesforce, and 40+ other sources from a single interface
- **Hybrid search** — Combine neural, keyword, and hybrid search methods with LLM reranking
- **Smart parameter tuning** — Automatic selection of search strategy based on your query intent
- **AI completions** — Get synthesized answers across all your connected data, not just raw results

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
> What did the team discuss about the product launch in Slack?
Claude searches your connected Slack workspace and synthesizes relevant messages.

> Find the API spec for our authentication service
Claude searches across GitHub repos, Confluence docs, and Jira tickets for implementation context.

> What's our current refund policy and when was it last updated?
Claude searches company wikis and Google Drive to find the authoritative document.

> Check Jira for open bugs related to payments
Claude queries your Jira project and compiles a summary of outstanding issues.

> Search our Google Drive for the Q4 board deck
Claude locates files across shared drives using semantic search.
```

## Supported Sources

Airweave connects to 40+ data sources including:

| Category | Sources |
|----------|---------|
| Productivity | Notion, Google Drive, Google Docs, Dropbox, OneDrive, SharePoint, Airtable |
| Communication | Slack, Gmail, Outlook, Microsoft Teams, Google Calendar |
| Project Management | Jira, Linear, Asana, Trello, Monday, ClickUp |
| Development | GitHub, GitLab, Bitbucket, Confluence |
| CRM & Sales | Salesforce, HubSpot, Zendesk, Pipedrive, Shopify |
| Data | PostgreSQL, Stripe |

See the full list at [docs.airweave.ai](https://docs.airweave.ai).

## Documentation

- [Airweave docs](https://docs.airweave.ai)
- [Airweave Cloud](https://app.airweave.ai)
- [GitHub](https://github.com/airweave-ai/airweave)
- [Discord](https://discord.gg/484HY9Ehxt)

## License

MIT
