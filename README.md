# Airweave Agent Skills

Reusable skills for AI agents that work with [Airweave](https://github.com/airweave-ai/airweave), the open-source context retrieval platform for AI agents.

## Installation

Install skills using the [skills CLI](https://skills.sh/docs):

```bash
npx skills add airweave-ai/skills
```

## Available Skills

### `airweave-setup`

**For developers integrating Airweave into their applications.**

Helps coding agents when users want to:
- Install Airweave (cloud or self-hosted)
- Create collections and connect data sources
- Set up the SDK (Python/TypeScript)
- Configure MCP servers
- Integrate Airweave into their application

```
airweave-setup/
├── SKILL.md           # Core setup instructions
├── SDK-REFERENCE.md   # Python & TypeScript SDK guide
├── MCP-SETUP.md       # MCP server configuration
└── SEARCH-PATTERNS.md # Search code patterns
```

---

### `airweave-search`

**For agents using Airweave MCP to retrieve context.**

Helps agents when users ask about their data in connected apps (Slack, GitHub, Notion, Jira, Google Drive, Salesforce, databases, etc.) or need context to complete tasks:
- When to search vs use training data
- Query formulation strategies
- Parameter selection for different intents
- Result interpretation and presentation
- Handling no/poor results

```
airweave-search/
├── SKILL.md           # Core search decision-making
├── PARAMETERS.md      # Deep dive on all search parameters
└── EXAMPLES.md        # Real-world search scenarios
```

---

## Which Skill Do I Need?

| Use Case | Skill |
|----------|-------|
| "Help me set up Airweave" | `airweave-setup` |
| "How do I connect Slack to Airweave?" | `airweave-setup` |
| "How do I install the Airweave SDK?" | `airweave-setup` |
| "Find messages about the product launch" | `airweave-search` |
| "What did the team discuss in Slack?" | `airweave-search` |
| "Search my company docs for the refund policy" | `airweave-search` |
| "Check our Notion docs for the API spec" | `airweave-search` |
| "Look at the Jira ticket for context" | `airweave-search` |

## How Skills Work

Skills are **knowledge for AI agents**, not tools. They teach agents how to accomplish tasks effectively:

1. **Discovery**: Agent reads skill metadata at startup
2. **Trigger**: When user request matches skill description, agent loads full instructions
3. **Apply**: Agent follows skill guidance to help the user
4. **Execute**: Agent uses available tools (MCP, code execution) or guides the user

Skills work regardless of execution method:
- **With MCP**: Agent calls `search-{collection}` tool
- **With code execution**: Agent writes SDK code
- **Without tools**: Agent guides the user on what to do

## Resources

- **Airweave Documentation**: https://docs.airweave.ai
- **Airweave Cloud**: https://app.airweave.ai
- **GitHub**: https://github.com/airweave-ai/airweave
- **Discord**: https://discord.gg/484HY9Ehxt

## Contributing

To add or improve skills:

1. Fork this repository
2. Create/edit skill directories with `SKILL.md` files
3. Follow the [skill authoring guidelines](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)
4. Submit a pull request

## License

MIT License - see [LICENSE](LICENSE) for details.
