<div align="center">
  <img src="https://cashbff.com/icon.png" alt="CashBFF" width="88" height="88">
  <h1>CashBFF MCP Server</h1>
  <p><strong>Talk to your money inside Claude.</strong></p>
  <p>
    <a href="https://cashbff.com">Website</a> ·
    <a href="https://cashbff.com/security">Security</a> ·
    <a href="https://cashbff.com/privacy">Privacy</a> ·
    <a href="https://cashbff.com/terms">Terms</a>
  </p>
</div>

## What it is

CashBFF is a personal finance MCP (Model Context Protocol) server. You connect your bank through Plaid, then ask Claude (or any MCP client) about your money in plain language: your balances, what is coming up, whether you can afford something, and where your cash goes. It runs as a hosted remote server, so you connect once and start asking.

- **Website:** https://cashbff.com
- **MCP endpoint:** `https://api.cashbff.com/mcp`
- **Transport:** Streamable HTTP
- **Auth:** OAuth 2.0 with PKCE and Dynamic Client Registration (you sign in with your phone)

## Connect

CashBFF is a remote server. Most MCP clients connect through the `mcp-remote` bridge, which opens your browser for sign in and manages tokens for you.

```json
{
  "mcpServers": {
    "cashbff": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://api.cashbff.com/mcp"]
    }
  }
}
```

Clients that support remote HTTP with OAuth directly (such as Claude Desktop and the claude.ai connector) can use the native form:

```json
{
  "mcpServers": {
    "cashbff": {
      "type": "streamable-http",
      "url": "https://api.cashbff.com/mcp"
    }
  }
}
```

On first connect, your browser opens to sign in with your phone number and approve access. A CashBFF Talk subscription is required to connect, and you can start a free trial at [cashbff.com](https://cashbff.com).

## Tools

CashBFF exposes 17 tools: 8 that read your money and 9 that write. Every write previews the change and waits for your confirmation before anything happens.

### Read

| Tool | What it does |
| --- | --- |
| `get_my_money_snapshot` | A full overview: running balance, recurring, upcoming, and recent transactions. |
| `get_my_balances` | Your current cash on hand and card balances. |
| `get_my_upcoming` | Scheduled and recurring items coming up. |
| `get_my_runway` | How long your money lasts at the current pace. |
| `get_balance_on_date` | Your projected balance on a specific date. |
| `get_calendar_view` | A calendar of money events across a date range. |
| `search_my_transactions` | Find transactions by merchant, category, or amount. |
| `list_recurring_suggestions` | Recurring patterns CashBFF detected, ready for you to confirm. |

### Write (each asks for confirmation)

| Tool | What it does |
| --- | --- |
| `add_goal` | Add a savings or payoff goal. |
| `add_transaction_note` | Record an off-bank note, such as cash you handed someone. |
| `add_scheduled_transaction` | Add expected income or an expense on a date. |
| `edit_scheduled_transaction` | Edit an item you created. |
| `delete_scheduled_transaction` | Remove an item you created. |
| `mark_transaction_reimbursable` | Flag a transaction as reimbursable. |
| `confirm_recurring_suggestion` | Confirm a detected recurring pattern. |
| `add_recurring_stream` | Add a recurring income or expense stream. |
| `update_recurring` | Update a recurring item. |

## Safety and privacy

- You stay in control. Reads are read only, and every write shows you the change and waits for your yes.
- Bank transactions stay exactly as your bank reports them through Plaid. CashBFF reads and organizes, and your money stays where it is.
- Bank connections are handled by Plaid. CashBFF uses your data to answer your questions and nothing more.
- More detail lives on [security](https://cashbff.com/security), [privacy](https://cashbff.com/privacy), and [terms](https://cashbff.com/terms).

## Troubleshooting

- **The connector will not connect.** Confirm the URL is exactly `https://api.cashbff.com/mcp`. On first connect a browser window opens to sign in.
- **Sign-in.** You sign in with your phone number, then the one-time code sent to it. A CashBFF Talk subscription is required to connect; a free trial is available at [cashbff.com](https://cashbff.com).
- **"Authorization failed" or the connection drops.** Remove the connector and add it again to run a fresh sign-in. Access tokens refresh automatically once connected.
- **A tool returns "not authenticated."** Your session expired; reconnect the server.
- **No data appears.** Connect a bank through CashBFF first so there is data to read. Reviewers can use the provided test account, which is preloaded with sample data.
- **Still stuck?** Email [daksh@cashbff.com](mailto:daksh@cashbff.com).

## About

CashBFF is built by [Daksh Khanna](https://khannadaksh.com) (Khanna's LLC).

This repository is documentation for the hosted CashBFF MCP server. The server is hosted and maintained for you.
