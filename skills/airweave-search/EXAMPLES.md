# Search Examples

Real-world examples of effective Airweave search patterns.

## Example 1: Finding Recent Discussions

**User:** "What did the team say about the new pricing in Slack?"

**Approach:**
- Topic: pricing discussions
- Source hint: Slack
- Standard search should surface relevant conversations

**Search:**
```
mode: "classic"
query: "new pricing discussion feedback"
limit: 15
```

**Response pattern:**
> Based on recent Slack conversations, the team discussed the new pricing last week. Key points:
>
> 1. **Sarah (Sales)**: Concerned about enterprise tier pricing being too high compared to competitors
> 2. **Mike (Product)**: Suggested adding a mid-tier option
> 3. **Lisa (Customer Success)**: Shared that 3 customers asked about volume discounts
>
> The main consensus was to revisit the enterprise pricing before the Q2 launch.

---

## Example 2: Finding Documentation

**User:** "Where's the API authentication documentation?"

**Approach:**
- Looking for specific document
- Likely in docs/wiki source
- Classic search with moderate limit

**Search:**
```
mode: "classic"
query: "API authentication documentation guide"
limit: 10
```

**Response pattern:**
> I found the API authentication documentation in your Confluence:
>
> **[API Authentication Guide](link)** - Last updated 2 weeks ago
>
> It covers:
> - OAuth 2.0 flow
> - API key generation
> - Token refresh handling
>
> Would you like me to summarize any specific section?

---

## Example 3: Complex Analysis (Using Agentic)

**User:** "What's our refund policy and how does it compare to what customers are actually asking for?"

**Approach:**
- Requires reasoning across multiple sources (policy docs + customer communications)
- Use agentic mode for multi-step retrieval and analysis

**Search:**
```
mode: "agentic"
query: "refund policy and customer refund requests feedback"
thinking: true
```

**Response pattern:**
> Based on my analysis across your docs and support tickets:
>
> **Current Policy** (from your Notion wiki):
> - Within 30 days: Full refund, no questions asked
> - 30-60 days: Store credit or exchange
> - After 60 days: Case-by-case basis
>
> **What Customers Are Asking** (from Zendesk tickets):
> - 8 tickets in the last month requested extensions beyond 30 days
> - 3 customers specifically asked about partial refunds (not currently offered)
> - Common complaint: the 60-day case-by-case process is too slow
>
> The main gap is that customers want more flexibility in the 30-60 day window.

---

## Example 4: Exploring a Topic

**User:** "Show me what customers have been saying about shipping"

**Approach:**
- Exploration, not specific answer
- Want to see multiple perspectives
- Instant search with higher limit to browse

**Search:**
```
mode: "instant"
query: "customer feedback shipping delivery"
limit: 20
```

**Response pattern:**
> Here's what I found about customer shipping feedback:
>
> **From Zendesk (last week):**
> - 5 tickets about delayed international shipping
> - 2 complaints about packaging damage
>
> **From Slack #customer-feedback:**
> - Discussion about carrier switch improving delivery times
> - Note from CS team about tracking notification issues
>
> **From Salesforce notes:**
> - Enterprise client requested expedited shipping options
>
> Want me to dig deeper into any of these areas?

---

## Example 5: No Results - Refining

**User:** "Find the Jenkins deployment pipeline config"

**First search:**
```
mode: "classic"
query: "Jenkins deployment pipeline configuration"
```

**Result:** No matches or low scores

**Refinement approach:**
1. Broaden: try `"deployment pipeline CI/CD"`
2. Switch mode: try `instant` with `retrieval_strategy: "keyword"`
3. Check source: "Is GitHub/GitLab connected?"

**Follow-up:**
> I didn't find specific Jenkins configuration. Let me try a broader search...
>
> I found deployment-related docs in your GitHub:
> - `.github/workflows/deploy.yml` - GitHub Actions deployment
> - `docs/deployment-guide.md` - General deployment steps
>
> It looks like you might be using GitHub Actions rather than Jenkins. Would you like me to show the GitHub Actions configuration?

---

## Example 6: Quick Lookup

**User:** "What bugs were reported today?"

**Approach:**
- Quick lookup in issue tracker
- Instant search is fast enough

**Search:**
```
mode: "instant"
query: "bug report issue"
limit: 20
```

**Response pattern:**
> Here are the recent bugs I found:
>
> 1. **[JIRA-1234]** Login timeout on mobile - P1, assigned to Alex
> 2. **[JIRA-1235]** Dashboard charts not loading - P2, unassigned
> 3. **[JIRA-1236]** Typo in email template - P3, assigned to Sarah
>
> The P1 login issue seems most urgent. Want details on that one?

---

## Example 7: Cross-Source Research

**User:** "What do we know about Acme Corp?"

**Approach:**
- Entity search (company name)
- Likely spans CRM + communications + docs
- Classic mode handles this well

**Search:**
```
mode: "classic"
query: "Acme Corp customer account"
limit: 25
```

**Response pattern:**
> Here's what I found about Acme Corp across your systems:
>
> **Salesforce:**
> - Enterprise customer since 2023
> - $50k ARR, renews in March
> - Main contact: John Smith (CTO)
>
> **Slack:**
> - Recent discussion about their API integration needs
> - They requested a custom feature last month
>
> **Zendesk:**
> - 2 open support tickets
> - Generally positive sentiment in past interactions
>
> Would you like more detail on any of these?

---

## Example 8: Mode Escalation

When `classic` doesn't give good enough results, escalate to `agentic`:

**User:** "How does our authentication system work end-to-end?"

**First try (classic):**
```
mode: "classic"
query: "authentication system architecture end-to-end"
limit: 15
```

**Result:** Scattered results from different sources, no coherent picture

**Escalate to agentic:**
```
mode: "agentic"
query: "How does our authentication system work end-to-end, from login to token validation?"
thinking: true
```

**Result:** Agent iteratively searches across GitHub, Confluence, and Notion, connects the pieces, and provides a coherent explanation.

---

## Anti-Patterns (What Not to Do)

### Don't search for everything
```
query: "all documents"  // Too broad, won't return useful results
```

### Don't use agentic for simple lookups
```
// User: "What's the Jira ticket number for the login bug?"
mode: "agentic"  // Overkill — use instant or classic
mode: "instant"  // Correct — fast keyword lookup
```

### Don't ignore no-results signals
```
// First search: no results
// Bad: "I couldn't find anything"
// Good: "Let me try a broader search..." then actually try with different mode/query
```
