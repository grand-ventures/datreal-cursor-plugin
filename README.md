# Datreal Cursor Plugin

Official Cursor Marketplace plugin for **[Datreal](https://datreal.com)** — Baltic and Nordic company data via MCP.

Datreal indexes **4M+ companies** across **Latvia (LV), Lithuania (LT), Estonia (EE), Finland (FI), and Sweden (SE)**. Use it from Cursor to search firms, pull profiles and multi-year financials, inspect shareholders/UBOs, and run compliance checks (VAT, tax rating, sanctions, debt, blacklist).

Published by **[Grand Ventures](https://grand-ventures.com)** · License **MIT**

## Install

### Cursor Marketplace (recommended)

Once listed, install **Datreal** from the [Cursor Marketplace](https://cursor.com/marketplace) with one click. The plugin registers the public Datreal MCP server automatically.

### Manual install

1. Clone or download this repository:
   ```bash
   git clone https://github.com/grand-ventures/datreal-cursor-plugin.git
   ```
2. In Cursor, add the plugin from the local folder (or install from the GitHub repo URL when supported).
3. Confirm the MCP server `datreal` points at `https://datreal.com/api/mcp`.

No API key, tokens, or environment variables are required.

## MCP endpoint

| | |
| --- | --- |
| URL | `https://datreal.com/api/mcp` |
| Auth | None |
| Transport | HTTP MCP |

Plugin config (`mcp.json`):

```json
{
  "mcpServers": {
    "datreal": {
      "url": "https://datreal.com/api/mcp"
    }
  }
}
```

Full MCP docs: [datreal.com/en/api-docs#mcp-server](https://datreal.com/en/api-docs#mcp-server)

> A REST HTTP API is also available at [`https://datreal.com/api/public/v1`](https://datreal.com/api/public/v1). **This plugin uses MCP**, not the REST API.

## Tools

| Tool | Purpose |
| --- | --- |
| `search_companies` | Search by name, address, or regcode (LV/LT/EE/FI/SE) |
| `get_company_details` | Full company profile for a registration number |
| `get_company_financials` | Multi-year income statement and balance sheet data |
| `get_company_compliance` | VAT, tax rating, sanctions, debt, blacklist flags |
| `get_shareholders` | Shareholders, UBOs, and ownership-chain links |

Company endpoints are rate-limited at roughly **20 requests / 10 seconds**. Prefer a country code when names are ambiguous. Typical flow: `search_companies` → then details / financials / compliance / shareholders.

## Example prompts

- “What is the revenue of SIA Maxima Latvija?”
- “Is regcode 40003245752 sanctioned?”
- “Find Estonian companies named Bolt”
- “Show shareholders and UBOs for this Lithuanian company”
- “Compare last three years of financials for this Finnish firm”

The bundled skill `skills/datreal-company-lookup` guides the agent on when and how to call these tools.

## Links

- Product: [https://datreal.com](https://datreal.com)
- MCP docs: [https://datreal.com/en/api-docs#mcp-server](https://datreal.com/en/api-docs#mcp-server)
- This repository: [https://github.com/grand-ventures/datreal-cursor-plugin](https://github.com/grand-ventures/datreal-cursor-plugin)

## License

[MIT](./LICENSE) © 2026 Grand Ventures (`hello@grand-ventures.com`)
