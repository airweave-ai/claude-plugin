# Search Parameters Deep Dive

Detailed guidance on when and how to use each Airweave search parameter.

## Mode Selection

### `mode`
The search mode determines which search tier to use.

| Mode | How It Works | Best For |
|------|--------------|----------|
| `instant` | Direct vector search — embeds query and matches against indexed content | Fast lookups, simple queries, browsing |
| `classic` (default) | AI generates an optimized search plan, then executes it | Most searches — balances speed and quality |
| `agentic` | Full agent loop — iteratively searches, reads, and reasons | Complex questions, multi-step reasoning, analysis |

**Use `instant` for:**
- Quick document lookups
- Browsing/exploring a topic
- When speed is critical
- Simple keyword or concept matching

**Use `classic` for:**
- Most general searches (this is the default)
- Finding specific information
- When you want good results without waiting

**Use `agentic` for:**
- "Summarize...", "Compare...", "Analyze..." queries
- Questions that span multiple sources
- When the answer requires reasoning over multiple documents
- Complex questions where `classic` doesn't return good enough results

## Core Parameters

### `query` (required)
The search query text. Use natural language — Airweave uses semantic search.

**Good queries:**
- "customer feedback about shipping delays"
- "authentication implementation OAuth"
- "Q4 sales report"

**Avoid:**
- Single words without context ("pricing")
- Boolean operators ("pricing AND feedback") - use natural language instead
- Exact phrases in quotes (semantic search handles this)

### `limit`
Maximum number of results to return (1-1000, default: 100).

| Limit | Use Case |
|-------|----------|
| 3-5 | Quick precise answer, top matches only |
| 10-20 | Standard search, balance of coverage and relevance |
| 50-100 | Exploration, comprehensive research |
| 100+ | Analysis across large dataset |

**Note:** For agentic mode, `limit` is optional — the agent decides how many results to collect based on the query. Only set it if you want to cap the maximum.

**Tip:** Start with lower limits for faster responses. Increase if results seem incomplete.

### `offset`
Skip this many results (for pagination). Default: 0. Available in instant and classic modes only.

Use when browsing through large result sets:
- First page: `limit: 20, offset: 0`
- Second page: `limit: 20, offset: 20`
- Third page: `limit: 20, offset: 40`

## Mode-Specific Parameters

### `retrieval_strategy` (instant mode only)
How to match documents in instant search.

| Strategy | How It Works | Best For |
|----------|--------------|----------|
| `hybrid` (default) | Combines semantic + keyword | Most searches, best overall |
| `neural` | Pure semantic/meaning | Conceptual questions, synonyms |
| `keyword` | Text matching | Exact terms, names, codes, IDs |

**Use `hybrid` for:**
- General searches (default choice)
- When unsure which strategy is best

**Use `neural` for:**
- "Find documents about..." (conceptual)
- When exact words don't matter
- Synonym-heavy domains

**Use `keyword` for:**
- Searching for names ("John Smith")
- Error codes, IDs ("ERR-4032")
- Exact technical terms
- When semantic matching gives wrong results

### `thinking` (agentic mode only)
Enable extended thinking / chain-of-thought for the search agent (default: false).

**Enable when:**
- Complex queries that require multi-step reasoning
- Questions that need the agent to plan its search strategy carefully
- When standard agentic search doesn't find the right results

## Common Parameter Combinations

### Quick Lookup
```
mode: "instant"
query: "deployment checklist"
retrieval_strategy: "keyword"
limit: 10
```

### Standard Search
```
mode: "classic"
query: "customer feedback pricing"
limit: 20
```

### Deep Research
```
mode: "agentic"
query: "Summarize all decisions about the authentication redesign"
thinking: true
```

### Exact Term Search
```
mode: "instant"
query: "ERR-4032 timeout"
retrieval_strategy: "keyword"
limit: 10
```

### Browse / Explore
```
mode: "instant"
query: "API documentation"
retrieval_strategy: "hybrid"
limit: 50
```
