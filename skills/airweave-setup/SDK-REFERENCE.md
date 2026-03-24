# Airweave SDK Reference

Complete reference for the Airweave Python and TypeScript SDKs.

## Installation

```bash
# Python
pip install airweave-sdk

# TypeScript/JavaScript
npm install @airweave/sdk
```

## Client Initialization

### Python

```python
from airweave import AirweaveSDK

# Cloud (default)
client = AirweaveSDK(api_key="YOUR_API_KEY")

# Self-hosted
client = AirweaveSDK(
    api_key="YOUR_API_KEY",
    base_url="http://localhost:8001"
)
```

### TypeScript

```typescript
import { AirweaveSDKClient, AirweaveSDKEnvironment } from "@airweave/sdk";

// Cloud
const client = new AirweaveSDKClient({ apiKey: "YOUR_API_KEY" });

// Self-hosted
const client = new AirweaveSDKClient({
    apiKey: "YOUR_API_KEY",
    environment: AirweaveSDKEnvironment.Local
});
```

## Collections API

### Create Collection

**Python:**
```python
collection = client.collections.create(name="My Collection")
# Returns: Collection object with readable_id
```

**TypeScript:**
```typescript
const collection = await client.collections.create({ name: "My Collection" });
```

### List Collections

**Python:**
```python
collections = client.collections.list()
for c in collections:
    print(f"{c.name}: {c.readable_id}")
```

**TypeScript:**
```typescript
const collections = await client.collections.list();
```

### Get Collection

**Python:**
```python
collection = client.collections.get(readable_id="my-collection-abc123")
```

### Delete Collection

**Python:**
```python
client.collections.delete(readable_id="my-collection-abc123")
```

## Source Connections API

### Create Source Connection

**Python (API Key Auth):**
```python
source = client.source_connections.create(
    name="My Stripe Connection",
    short_name="stripe",
    readable_collection_id=collection.readable_id,
    authentication={
        "credentials": {"api_key": "sk_live_..."}
    }
)
```

**OAuth Sources (Google, Microsoft, etc.):**

OAuth sources require user authorization via the Airweave UI at https://app.airweave.ai or implementing the OAuth flow in your application. The SDK does not provide a direct OAuth helper—use the web UI for the simplest setup.

### Available Source Short Names

**Productivity & Docs:**
| Source | Short Name | Auth Type |
|--------|------------|-----------|
| Notion | `notion` | Integration Token |
| Google Drive | `google_drive` | OAuth |
| Google Docs | `google_docs` | OAuth |
| Google Slides | `google_slides` | OAuth |
| Dropbox | `dropbox` | OAuth |
| OneDrive | `onedrive` | OAuth |
| SharePoint | `sharepoint` | OAuth |
| Box | `box` | OAuth |
| Airtable | `airtable` | API Key |
| Excel | `excel` | OAuth |
| Word | `word` | OAuth |
| OneNote | `onenote` | OAuth |

**Communication:**
| Source | Short Name | Auth Type |
|--------|------------|-----------|
| Slack | `slack` | OAuth |
| Gmail | `gmail` | OAuth |
| Outlook Mail | `outlook_mail` | OAuth |
| Outlook Calendar | `outlook_calendar` | OAuth |
| Google Calendar | `google_calendar` | OAuth |
| Teams | `teams` | OAuth |

**Project Management:**
| Source | Short Name | Auth Type |
|--------|------------|-----------|
| Jira | `jira` | API Token |
| Linear | `linear` | API Key |
| Asana | `asana` | OAuth |
| Trello | `trello` | API Key |
| Monday | `monday` | API Key |
| ClickUp | `clickup` | OAuth |
| Todoist | `todoist` | OAuth |

**Development:**
| Source | Short Name | Auth Type |
|--------|------------|-----------|
| GitHub | `github` | Personal Access Token / OAuth |
| GitLab | `gitlab` | Personal Access Token |
| Bitbucket | `bitbucket` | OAuth |
| Confluence | `confluence` | API Token |

**CRM & Sales:**
| Source | Short Name | Auth Type |
|--------|------------|-----------|
| Salesforce | `salesforce` | OAuth |
| HubSpot | `hubspot` | OAuth |
| Attio | `attio` | API Key |
| Zendesk | `zendesk` | OAuth |
| Pipedrive | `pipedrive` | OAuth |
| Zoho CRM | `zoho_crm` | OAuth |
| Shopify | `shopify` | OAuth |

**Data:**
| Source | Short Name | Auth Type |
|--------|------------|-----------|
| Stripe | `stripe` | API Key |
| PostgreSQL | `postgresql` | Connection String |

### List Source Connections

**Python:**
```python
sources = client.source_connections.list(
    readable_collection_id=collection.readable_id
)
```

### Trigger Sync

**Python:**
```python
client.source_connections.sync(
    source_connection_id=source.id
)
```

## Search API

Airweave provides three search modes: **instant**, **classic**, and **agentic**.

### Instant Search

Direct vector search. Fastest mode.

**Python:**
```python
results = client.collections.search(
    readable_id=collection.readable_id,
    query="Find customer complaints about shipping",
    mode="instant",
    retrieval_strategy="hybrid",  # "hybrid" (default), "neural", "keyword"
    limit=20,
    offset=0
)
```

**TypeScript:**
```typescript
const results = await client.collections.search(
    collection.readableId,
    {
        query: "Find customer complaints about shipping",
        mode: "instant",
        retrievalStrategy: "hybrid",
        limit: 20
    }
);
```

### Classic Search

AI-optimized search with LLM-generated search plans. Default mode.

**Python:**
```python
results = client.collections.search(
    readable_id=collection.readable_id,
    query="API documentation",
    mode="classic",
    limit=20,
    offset=0
)
```

### Agentic Search

Full agent loop with iterative reasoning.

**Python:**
```python
results = client.collections.search(
    readable_id=collection.readable_id,
    query="Summarize the key decisions from last week's product meetings",
    mode="agentic",
    thinking=True,   # Enable extended reasoning
    limit=10         # Optional — agent decides if not set
)
```

### Search Parameters

| Parameter | Type | Modes | Description |
|-----------|------|-------|-------------|
| `query` | string | All | Search query (required) |
| `mode` | string | All | `"instant"`, `"classic"` (default), `"agentic"` |
| `limit` | number | All | Max results (1-1000, default: 100) |
| `offset` | number | instant, classic | Skip results for pagination (default: 0) |
| `retrieval_strategy` | string | instant | `"hybrid"` (default), `"neural"`, `"keyword"` |
| `thinking` | boolean | agentic | Enable extended reasoning (default: false) |
| `filter` | list | All | Filter groups for narrowing results |

## Search Result Structure

```python
results.results  # List of SearchResult objects

# Each result contains:
{
    "entity_id": "page-abc123",
    "name": "Production Deployment Guide",
    "relevance_score": 0.94,
    "textual_representation": "# Production Deployment Guide\n\nThis document covers...",
    "breadcrumbs": [
        {"entity_id": "ws-1", "name": "Acme Workspace", "entity_type": "NotionWorkspaceEntity"},
        {"entity_id": "db-eng", "name": "Engineering", "entity_type": "NotionDatabaseEntity"}
    ],
    "airweave_system_metadata": {
        "source_name": "notion",
        "entity_type": "NotionPageEntity",
        "original_entity_id": "page-abc123",
        "chunk_index": 0,
        "sync_id": "d4e5f6a7-...",
        "sync_job_id": "a1b2c3d4-..."
    },
    "access": {"viewers": null, "is_public": null},
    "web_url": "https://notion.so/Deployment-Guide-abc123",
    "url": null,
    "created_at": "2025-02-10T09:15:00Z",
    "updated_at": "2025-03-18T16:30:00Z",
    "raw_source_fields": {
        "icon": "...",
        "archived": false
    }
}
```

### Key Result Fields

| Field | Description |
|-------|-------------|
| `entity_id` | Unique ID of the entity in the source |
| `name` | Display name / title |
| `relevance_score` | Relevance score (0-1, higher = more relevant) |
| `textual_representation` | Full text content |
| `breadcrumbs` | Hierarchy path (e.g., Workspace > Database > Page) |
| `airweave_system_metadata.source_name` | Source app (e.g., "slack", "notion") |
| `web_url` | Link to view in the source application |
| `url` | Download URL (for file entities only) |
| `created_at` / `updated_at` | Timestamps |
| `raw_source_fields` | Source-specific metadata |

## Error Handling

**Python:**
```python
try:
    results = client.collections.search(
        readable_id="invalid-id",
        query="test"
    )
except Exception as e:
    error_msg = str(e)
    if "404" in error_msg:
        print("Collection not found")
    elif "422" in error_msg:
        print("Invalid request (e.g., collection has no sources)")
    elif "401" in error_msg or "403" in error_msg:
        print("Invalid API key or access denied")
    else:
        print(f"API error: {e}")
```

## Rate Limits

- The API has rate limits for protection
- Implement exponential backoff for production use
- Contact support for higher limits if needed

```python
import time

def search_with_retry(client, collection_id, query, max_retries=3):
    for attempt in range(max_retries):
        try:
            return client.collections.search(
                readable_id=collection_id,
                query=query
            )
        except Exception as e:
            if "429" in str(e) and attempt < max_retries - 1:
                time.sleep(2 ** attempt)
            else:
                raise
```
