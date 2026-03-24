# Airweave MCP Server Setup

Configure the Airweave MCP server to enable AI assistants to search your data.

## Prerequisites

1. An Airweave account with API key
2. A collection with synced data
3. Node.js 18+ (for local mode)

## Deployment Modes

### 1. Local Mode (Claude Desktop, Cursor)

For desktop AI assistants that run MCP servers locally.

#### Installation via npx (Recommended)

No installation required—runs directly:

```bash
npx airweave-mcp-search
```

#### Configuration for Cursor

Create or edit `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "airweave-search": {
      "command": "npx",
      "args": ["airweave-mcp-search"],
      "env": {
        "AIRWEAVE_API_KEY": "your-api-key-here",
        "AIRWEAVE_COLLECTION": "your-collection-readable-id",
        "AIRWEAVE_BASE_URL": "https://api.airweave.ai"
      }
    }
  }
}
```

#### Configuration for Claude Desktop

Add to `~/.config/claude/claude_desktop_config.json` (macOS/Linux) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "airweave-search": {
      "command": "npx",
      "args": ["airweave-mcp-search"],
      "env": {
        "AIRWEAVE_API_KEY": "your-api-key-here",
        "AIRWEAVE_COLLECTION": "your-collection-readable-id",
        "AIRWEAVE_BASE_URL": "https://api.airweave.ai"
      }
    }
  }
}
```

#### Local Development (Self-Hosted Airweave)

For testing with a local Airweave instance:

```json
{
  "mcpServers": {
    "airweave-search": {
      "command": "node",
      "args": ["/path/to/airweave/mcp/build/index.js"],
      "env": {
        "AIRWEAVE_API_KEY": "your-local-api-key",
        "AIRWEAVE_COLLECTION": "your-collection-id",
        "AIRWEAVE_BASE_URL": "http://localhost:8001"
      }
    }
  }
}
```

### 2. Hosted Mode (Cloud AI Platforms)

For cloud-based AI platforms like OpenAI Agent Builder.

- **URL**: `https://mcp.airweave.ai`
- **Transport**: Streamable HTTP (MCP 2025-03-26)

Pass your API key via headers:
- `Authorization: Bearer YOUR_API_KEY`
- or `X-API-Key: YOUR_API_KEY`

## Available Tools

Once configured, the MCP server exposes these tools:

### `search-{collection}`

Search within your Airweave collection using one of three modes.

**Parameters:**

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | (required) | Search query text |
| `mode` | string | "classic" | Search mode: "instant", "classic", or "agentic" |
| `limit` | number | 100 | Max results (1-1000) |
| `offset` | number | 0 | Skip results for pagination (instant/classic only) |
| `retrieval_strategy` | string | "hybrid" | "hybrid", "neural", "keyword" (instant mode only) |
| `thinking` | boolean | false | Enable extended reasoning (agentic mode only) |

**Search Modes:**

| Mode | Description |
|------|-------------|
| `instant` | Direct vector search — fastest, best for simple lookups |
| `classic` | AI-optimized search with LLM-generated plans — default, best for most searches |
| `agentic` | Full agent loop with iterative reasoning — best for complex analysis |

**Example Usage:**

```
search-my-collection({
  query: "customer complaints about shipping delays",
  mode: "classic",
  limit: 10
})
```

```
search-my-collection({
  query: "deployment checklist",
  mode: "instant",
  retrieval_strategy: "keyword",
  limit: 5
})
```

```
search-my-collection({
  query: "Summarize decisions about the Q4 roadmap",
  mode: "agentic",
  thinking: true
})
```

### `get-config`

Shows current server configuration.

```
get-config()
```

## Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `AIRWEAVE_API_KEY` | Yes | - | Your Airweave API key |
| `AIRWEAVE_COLLECTION` | Yes | - | Collection readable ID |
| `AIRWEAVE_BASE_URL` | No | `https://api.airweave.ai` | API base URL |

## Usage Examples

Once the MCP server is configured, you can ask your AI assistant:

**Basic Searches (classic mode):**
- "Search for customer feedback about pricing"
- "Find documents related to API documentation"
- "Look up information about data privacy policies"

**Quick Lookups (instant mode):**
- "Quickly find the deployment checklist"
- "Look up error code ERR-4032"

**Deep Analysis (agentic mode):**
- "Analyze what customers are saying about shipping across all channels"
- "Summarize the key decisions from last week's meetings"
- "Compare our current auth implementation with what was discussed in the RFC"

**Natural Language Mapping:**
- "Find the first 5 results" → `limit: 5`
- "Show me results 11-20" → `limit: 10, offset: 10`
- "Use keyword search" → `mode: "instant", retrieval_strategy: "keyword"`
- "Do a deep search" → `mode: "agentic"`

## Troubleshooting

### "Error: AIRWEAVE_API_KEY environment variable is required"

Ensure the API key is set in your MCP configuration. Verify the key is valid at https://app.airweave.ai.

### "Error: AIRWEAVE_COLLECTION environment variable is required"

Set the collection's readable ID (find it in your Airweave dashboard or via the API).

### "Airweave API error (404)"

- Verify the collection ID is correct
- Check the collection exists and you have access

### "Airweave API error (401)"

- Check your API key is valid and not expired
- Ensure the API key has permissions for this collection

### "Airweave API error (422)"

- The collection may have no sources connected — add at least one source
- Source credentials may have expired — reconnect in the Airweave UI

### MCP Server Not Appearing

1. Restart your AI client (Cursor/Claude Desktop)
2. Check the MCP config file for JSON syntax errors
3. Verify the npx command works: `npx airweave-mcp-search --version`

### Debug Mode

The server logs to stderr. Check logs for:
- "Airweave MCP Search Server started"
- "Collection: your-collection-id"
- "Base URL: https://api.airweave.ai"

## Multiple Collections

To search multiple collections, configure multiple MCP servers:

```json
{
  "mcpServers": {
    "airweave-engineering": {
      "command": "npx",
      "args": ["airweave-mcp-search"],
      "env": {
        "AIRWEAVE_API_KEY": "your-api-key",
        "AIRWEAVE_COLLECTION": "engineering-docs-abc123"
      }
    },
    "airweave-support": {
      "command": "npx",
      "args": ["airweave-mcp-search"],
      "env": {
        "AIRWEAVE_API_KEY": "your-api-key",
        "AIRWEAVE_COLLECTION": "support-tickets-xyz789"
      }
    }
  }
}
```

Each collection will have its own search tool: `search-engineering-docs-abc123` and `search-support-tickets-xyz789`.
