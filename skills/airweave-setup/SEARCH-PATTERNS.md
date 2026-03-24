# Airweave Search Patterns

Common search patterns and recipes for effective data retrieval.

## Choosing the Right Mode

| Scenario | Mode | Why |
|----------|------|-----|
| Simple lookup | `instant` | Fast, direct vector match |
| Most searches | `classic` | AI-optimized, good balance |
| Complex analysis | `agentic` | Iterative reasoning |
| Keyword/ID search | `instant` + `keyword` strategy | Exact text matching |
| "Summarize..." / "Compare..." | `agentic` | Needs reasoning |

## Search Recipes

### High-Precision Search

For finding exact, high-quality matches:

```python
results = client.collections.search(
    readable_id=collection_id,
    query="authentication implementation guide",
    mode="instant",
    retrieval_strategy="keyword",
    limit=10
)
```

### High-Recall Search

For finding all potentially relevant documents:

```python
results = client.collections.search(
    readable_id=collection_id,
    query="user onboarding",
    mode="classic",
    limit=100
)
```

### Deep Research

For complex questions that require reasoning across sources:

```python
results = client.collections.search(
    readable_id=collection_id,
    query="How has our authentication architecture evolved and what are the current pain points?",
    mode="agentic",
    thinking=True
)
```

### Quick Lookup

For fast, simple searches:

```python
results = client.collections.search(
    readable_id=collection_id,
    query="deployment checklist",
    mode="instant",
    limit=10
)
```

### Balanced Search

Good default for most use cases:

```python
results = client.collections.search(
    readable_id=collection_id,
    query="API documentation",
    mode="classic",
    limit=25
)
```

## Pagination Pattern

For browsing large result sets (instant and classic modes):

```python
def paginated_search(client, collection_id, query, page_size=20):
    """Generator for paginated search results."""
    offset = 0
    while True:
        results = client.collections.search(
            readable_id=collection_id,
            query=query,
            mode="classic",
            limit=page_size,
            offset=offset
        )

        if not results.results:
            break

        yield from results.results

        if len(results.results) < page_size:
            break

        offset += page_size

# Usage
for result in paginated_search(client, collection_id, "customer feedback"):
    print(f"{result.name}: {result.textual_representation[:100]}")
```

## Mode Escalation Pattern

Start with a fast mode and escalate if results are insufficient:

```python
# Try classic first
results = client.collections.search(
    readable_id=collection_id,
    query="authentication architecture",
    mode="classic",
    limit=20
)

# If results are poor, escalate to agentic
if not results.results or results.results[0].relevance_score < 0.5:
    results = client.collections.search(
        readable_id=collection_id,
        query="authentication architecture",
        mode="agentic",
        thinking=True
    )
```

## Multi-Query Fusion

Search with multiple queries and combine results:

```python
queries = [
    "password reset flow",
    "forgot password implementation",
    "user credential recovery"
]

all_results = []
seen_ids = set()

for query in queries:
    results = client.collections.search(
        readable_id=collection_id,
        query=query,
        mode="instant",
        limit=10
    )

    for result in results.results:
        if result.entity_id not in seen_ids:
            all_results.append(result)
            seen_ids.add(result.entity_id)

# Sort by score
all_results.sort(key=lambda x: x.relevance_score, reverse=True)
```

## MCP Tool Usage Patterns

When using Airweave via MCP, the AI assistant translates natural language to parameters:

| User Request | MCP Parameters |
|--------------|----------------|
| "Find recent bug reports" | `query: "bug reports", mode: "classic"` |
| "Get the top 5 results" | `limit: 5` |
| "Summarize the meeting notes" | `mode: "agentic"` |
| "Find exact matches for ERR-4032" | `mode: "instant", retrieval_strategy: "keyword"` |
| "Deep dive into our auth system" | `mode: "agentic", thinking: true` |
| "Quick lookup for the deploy guide" | `mode: "instant"` |

## Performance Tips

1. **Start with `classic`**: It's the best default for most searches
2. **Use `instant` for speed**: When latency matters more than result quality
3. **Reserve `agentic` for complexity**: It's slower but handles multi-step reasoning
4. **Use appropriate limits**: Start with smaller limits (10-25) for interactive use
5. **Use `keyword` strategy for exact terms**: Names, IDs, error codes in instant mode
