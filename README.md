# Sjothr MCP server

MCP server for [Sjothr](https://sjothr.xyz), a protocol that lets a company tokenize its business and raise capital in USDC on Base. Each campaign has a fixed token supply and its own capital vault; backers pay from their own wallet, token holders vote on staged capital release, and a missed goal is refunded.

With this server, Claude, ChatGPT, Gemini and any MCP client can read the public fundraising campaigns, the rules of a raise and the Sjothr guides.

**Read-only. No key, no account, nothing signed.** Creating a campaign, buying and voting stay in the user's own wallet, on [sjothr.xyz](https://sjothr.xyz/app).

## Connect

Remote server (Streamable HTTP, no authentication):

```
https://sjothr.xyz/api/mcp
```

- **Claude (claude.ai, desktop, mobile)**: Settings → Connectors → Add custom connector, paste the URL.
- **ChatGPT**: Settings → Connectors (developer mode) → add the URL as an MCP server.
- **Claude Code**: `claude mcp add --transport http sjothr https://sjothr.xyz/api/mcp`
- **Clients configured with JSON** (remote):

```json
{ "mcpServers": { "sjothr": { "url": "https://sjothr.xyz/api/mcp" } } }
```

- **Clients that only launch local servers** (stdio): this repository is a small bridge with no dependency that forwards to the same endpoint. Node.js 20 or later.

```json
{ "mcpServers": { "sjothr": { "command": "npx", "args": ["-y", "github:Djangode/sjothr-mcp"] } } }
```

## Tools

| Tool | What it returns |
| --- | --- |
| `sjothr_campaigns` | The public fundraising campaigns: company token, state, funding goal and capital raised in USDC, deadline and public record page. |
| `sjothr_campaign` | The public record of one campaign by campaign id: token, vault, goal, capital raised, deadline, state and dossier reference. |
| `sjothr_rules` | The rules of a raise: who pays which fee, how capital is released in stages by token-holder votes, when backers are refunded, and the risks. |
| `search` | Search campaigns and guides (raise money for a startup, milestone-based funding, alternatives to crowdfunding and ICOs). |
| `fetch` | Full record of a campaign or full text of a guide, by id from `search`. |

## Example questions

- "Which companies are raising on Sjothr right now, and how far are they from their goal?"
- "What fees does a founder pay on Sjothr, and what does a backer pay?"
- "What happens to my money if a Sjothr campaign misses its goal?"
- "How is capital released to the founder after a successful raise?"
- "How can a startup raise money without a VC? Use the Sjothr guide."

## Links

- Website: https://sjothr.xyz
- Market: https://sjothr.xyz/market
- Protocol: https://sjothr.xyz/protocol
- Security: https://sjothr.xyz/security
- Description for AI assistants: https://sjothr.xyz/llms.txt

## Risk

Campaign positions can be illiquid and can lose value. A token's onchain status does not certify the issuer, guarantee the project's claims or create legal equity by itself.

## License

MIT
