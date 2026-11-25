# Sjothr MCP server

[![Djangode/sjothr-mcp MCP server](https://glama.ai/mcp/servers/Djangode/sjothr-mcp/badges/score.svg)](https://glama.ai/mcp/servers/Djangode/sjothr-mcp) [![Sjothr MCP connector](https://glama.ai/mcp/connectors/io.github.Djangode/sjothr-mcp/badges/score.svg)](https://glama.ai/mcp/connectors/io.github.Djangode/sjothr-mcp)

MCP server for [Sjothr](https://sjothr.xyz), a protocol that lets a company tokenize its business and raise capital in USDC on Base. Each campaign has a fixed token supply and its own capital vault; backers pay from their own wallet, token holders vote on staged capital release, and a missed goal is refunded.

Sjothr also runs a credit risk market: credit companies publish the risk of their loan books in tranches, and guarantors cover a tranche for a yearly premium.

With this server, Claude, ChatGPT, Gemini and any MCP client read the raises, the credit risk portfolios, the Market and the guides, and get the unsigned transactions of an action (invest, cover a tranche, collect a premium, buy or sell a position, resell startup tokens, vote, refund).

**No key, no account, nothing signed by the server.** Every prepared transaction is signed and sent by the user's or the agent's own wallet; creating a campaign or publishing a portfolio stays on [sjothr.xyz](https://sjothr.xyz/app).

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
| `sjothr_prepare` | Unsigned transactions to invest in a raise, from the contract's quote (allowance, then purchase). |
| `sjothr_risk_portfolios` | Verified credit risk portfolios: company first loss, tranches, premium, protection dates, lending of the guarantees. |
| `sjothr_risk_portfolio` | One portfolio with the open offers of each tranche and the positions for sale. |
| `sjothr_risk_positions` | Guarantor positions of a wallet: amount covered, premium and interest to collect, sale price. |
| `sjothr_risk_prepare` | Unsigned transactions to cover a tranche, withdraw an offer, collect, sell, unlist, buy or release a position. |
| `sjothr_market` | Resale of startup tokens before their listing: offers and requests at the price of the current round. |
| `sjothr_market_prepare` | Unsigned transactions to sell, buy, validate an exchange, cancel or collect on the Market, checked against the contract. |
| `sjothr_backer` | A raise seen from a wallet: rounds, proposed round and its vote, kept tokens. |
| `sjothr_backer_prepare` | Unsigned transaction to vote on a round or a capital release, claim a refund or redeem the remaining capital. |
| `search` | Search campaigns, credit risk portfolios and guides (raise money for a startup, milestone-based funding, credit risk). |
| `fetch` | Full record of a campaign or a portfolio, or full text of a guide, by id from `search`. |

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
