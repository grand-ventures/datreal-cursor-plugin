---
name: datreal-company-lookup
description: Look up Baltic and Nordic company profiles, financials, shareholders, and compliance via Datreal. Use when the user asks about company data in LV, LT, EE, FI, or SE, sanctions/tax compliance, ownership/UBOs, or firmographics.
---

# Datreal company lookup

Use the Datreal MCP server to look up companies across Latvia (LV), Lithuania (LT), Estonia (EE), Finland (FI), and Sweden (SE). No API key is required.

Docs: [Datreal MCP server](https://datreal.com/en/api-docs#mcp-server)

## When to use

- Company search by name, address, or registration number
- Firmographics / legal profile for a known regcode
- Multi-year financial statements
- Compliance: VAT, tax rating, sanctions, tax debt, blacklist
- Ownership / shareholders / UBOs and ownership chains

## Tools

### `search_companies`

Search by name, address, or regcode across LV/LT/EE/FI/SE.

| Argument | Required | Notes |
| --- | --- | --- |
| `query` | yes | Min. 2 characters |
| `country` | no | `LV`, `LT`, `EE`, `FI`, or `SE` — prefer when known |
| `limit` | no | 1–100, default 10 |

Returns basic info: name, regcode, address, status, and last-year financials.

### `get_company_details`

Full company profile for a registration number.

| Argument | Required | Notes |
| --- | --- | --- |
| `regcode` | yes | Company registration number |
| `countryCode` | no | Recommended to disambiguate |

Returns name, address, legal form, status, registration dates, VAT, tax rating, NACE, sanctions/compliance flags, and a latest financial summary.

### `get_company_financials`

Multi-year income statement and balance sheet data.

| Argument | Required | Notes |
| --- | --- | --- |
| `regcode` | yes | |
| `countryCode` | no | Recommended |

Returns revenue, profit, assets, equity, employees, liabilities, cash flow, and more per fiscal year.

### `get_company_compliance`

Compliance and risk indicators.

| Argument | Required | Notes |
| --- | --- | --- |
| `regcode` | yes | |
| `countryCode` | no | Recommended |

Returns `vatNumber`, `vatStatus`, `taxRating`, `isSanctioned`, `hasTaxDebt`, `isBlacklisted`, and related flags.

### `get_shareholders`

Shareholders and beneficial owners (UBOs).

| Argument | Required | Notes |
| --- | --- | --- |
| `regcode` | yes | |
| `countryCode` | no | Recommended |
| `legalForm` | no | e.g. `SIA`, `AS` — improves stockholder vs member classification |

Returns name, ownership %, share count/value, entity type, country, and (when applicable) the shareholder’s own regcode for further ownership-chain lookups.

## Recommended workflow

1. Start with `search_companies` unless the user already gave a regcode.
2. Prefer a country code (`country` / `countryCode`) when disambiguating common names.
3. Pick the best match, then call:
   - `get_company_details` for profile
   - `get_company_financials` for statements
   - `get_company_compliance` for sanctions/tax/VAT risk
   - `get_shareholders` for ownership; follow legal-entity shareholder regcodes to walk the chain
4. Cite regcode, country, and company name in the answer.

## Constraints

- No API key or auth headers — the MCP endpoint is public.
- Respect rate limits: company endpoints are roughly **20 requests / 10 seconds**. Batch thoughtfully; do not hammer lookups in a tight loop.
- Covered countries only: **LV, LT, EE, FI, SE**.

## Example prompts this skill should handle

- “What is the revenue of SIA Maxima Latvija?”
- “Is regcode 40003245752 sanctioned?”
- “Find Estonian companies named Bolt”
- “Who owns this Lithuanian company and what is their tax rating?”
