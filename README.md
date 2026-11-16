# ReviewTactic – Google Review Checker (MCP server)

A free remote MCP server that lets an AI assistant check a business's Google reviews and local
ranking. No account, no API key, nothing to install.

- **Server URL:** `https://api.reviewtactic.com/functions/v1/mcp`
- **Transport:** Streamable HTTP (stateless)
- **Authentication:** none
- **Docs:** https://reviewtactic.com/mcp/
- **Privacy:** https://reviewtactic.com/privacy/#mcp-server
- **Contact:** hello@reviewtactic.com
- **Author:** [Adam Palicz](https://reviewtactic.com/resources/authors/adam-palicz/), co-founder of ReviewTactic ([LinkedIn](https://www.linkedin.com/in/adampalicz/))

This repository holds the documentation and the registry file. The server itself runs on
ReviewTactic's infrastructure.

## Tools

| Tool | What it does | Changes anything? |
|---|---|---|
| `find_business` | Finds a business on Google Maps by name and town. Returns up to 5 matches with place id, address, rating, review count and coordinates. | No (read-only) |
| `check_negative_reviews` | Screens the business's negative Google reviews against Google's review policies. Returns counts, the policy categories that matched and up to 5 short excerpts of flagged reviews. | No (read-only) |
| `check_local_rank` | Checks where the business appears in Google Maps results for one search phrase from one point, and up to 5 competitors ranked above it. | No (read-only) |
| `email_removal_report` | Emails the written report for an already checked business to an address the user typed. | Sends one email. Marked so that assistants ask for confirmation first. |

A flagged review is one that may break a Google policy. It is an automated first check, not a
promise that Google will remove it.

## Add it to your assistant

**Claude (claude.ai, desktop, mobile)**

1. Open Settings, then Connectors.
2. Choose Add custom connector.
3. Paste the server URL and save.

**ChatGPT**

1. Open Settings, then Apps & Connectors.
2. Turn on developer mode.
3. Add a connector, paste the server URL and choose "No authentication".

**Claude Code**

```bash
claude mcp add --transport http reviewtactic https://api.reviewtactic.com/functions/v1/mcp
```

**Cursor** (`~/.cursor/mcp.json`)

```json
{ "mcpServers": { "reviewtactic": { "url": "https://api.reviewtactic.com/functions/v1/mcp" } } }
```

## Example prompts

- "Check Joe's Pizza in Austin for negative Google reviews that break Google's rules."
- "Where does Joe's Pizza rank on Google Maps for 'pizza' in Austin?"
- "Email me the list of reviews you flagged."

## Limits

- A finished check is reused for 7 days, so repeat checks of the same business are instant.
- Fresh checks share a daily allowance across all users. When it is used up, the server says so and
  the same checks stay free at https://reviewtactic.com/free-tools/.
- A first check of a business can take up to a minute. If the answer says it is still running, ask
  again shortly.

## Data

The server reads public Google data about the business you name. It keeps a usage record per call
(tool, time, calling address for 30 days). An email address is stored only when you ask for the
report by email. Details: https://reviewtactic.com/privacy/#mcp-server

## Who is behind it

The server is built and maintained by [Adam Palicz](https://reviewtactic.com/resources/authors/adam-palicz/),
co-founder of [ReviewTactic](https://reviewtactic.com/), where he leads product development.
ReviewTactic makes Google review tools for local businesses. Questions and bug reports:
hello@reviewtactic.com.
