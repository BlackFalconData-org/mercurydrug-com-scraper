# Mercury Drug Generic Medicine Scraper

Extract structured data from [mercurydrug.com](https://mercurydrug.com) — Search the Mercury Drug Philippines generic medicine directory by name or ingredient and get generic names, synonyms, drug class and Prescribing Information Sheet PDF links, with scheduled runs that return only new or changed entries.

**[Mercury Drug Generic Medicine Scraper on Apify →](https://apify.com/blackfalcondata/mercurydrug-com-scraper?fpr=1h3gvi)**

---

## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.

## Key features

**Search with filters** — Search by keyword. Filter by 📝 description format, and more.

**Detail enrichment** — Fetch structured metadata for each product.

**Incremental mode** — Only get new or changed products since your last run. Content hash per product — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run duplicate detection across runs. Build audit trails of how products evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Result cap** — Stop after N products (up to 1.000). Set to 0 for the full catalog.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every product returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases

**Data pipeline automation**
Integrate with your ETL pipeline to collect structured products from mercurydrug.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor products, track trends, and analyze market dynamics with structured, deduplicated data from mercurydrug.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired ${NOUNS}. Perfect for price-tracking, churn analysis, and alerting pipelines.

**AI / LLM training data**
Structured JSON per product is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "query": "paracetamol",
  "maxResults": 100,
  "includeDetails": true,
  "compact": false,
  "excludeEmptyFields": false,
  "incrementalMode": false,
  "skipReposts": false,
  "notifyOnlyChanges": false,
  "notificationLimit": 5,
  "descriptionFormat": "all"
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | string | — | Generic drug or active-ingredient name to search for (substring match, at least 3 characters) — e.g. "paracetamol", "amoxicillin". Paste a JSON array to search several at once, e.g. ["paracetamol","ibuprofen"]. |
| `maxResults` | integer | `100` | Maximum total results (0 = unlimited). |
| `startUrls` | array | `[]` | Paste raw generic-search URLs from mercurydrug.com (e.g. https://www.mercurydrug.com/cgi-bin/drugsearch?searchtype=Generic&searchitem=paracetamol). Each URL becomes its own search task; results are merged and deduped. When provided, startUrls REPLACE the query field (unsupported URLs are skipped, and if none is supported the run stops with an input error). |
| `includeDetails` | boolean | `true` | Fetch each generic's detail page for synonyms, drug class and the Prescribing Information Sheet (PDF) link. Turn off for a names-only index (faster, fewer fields). |
| `compact` | boolean | `false` | Core fields only (for AI-agent/MCP workflows). |
| `excludeEmptyFields` | boolean | `false` | Drop null, empty-string, and empty-array fields from each record before push. Smaller payloads for AI agents and dashboards. |
| `incrementalMode` | boolean | `false` | Compare against previous run state. stateKey is optional — defaults to a value derived from search inputs (queries, startUrls) so different searches never share state. |
| `stateKey` | string | — | Optional. Stable identifier for the tracked search universe. Leave empty to auto-generate from search inputs. |
| `emitUnchanged` | boolean | `false` | When incremental, also emit generic-drug entries that have not changed since the last run (marked UNCHANGED). |
| `emitExpired` | boolean | `false` | When incremental, also emit previously seen entries that are no longer found (marked EXPIRED). Scans the complete result set to decide. |
| `skipReposts` | boolean | `false` | When incremental, skip entries whose content matches an entry that disappeared in a prior run (cross-run duplicate detection). |
| `notifyOnlyChanges` | boolean | `false` | When Incremental Mode is on, only send notifications for NEW and UPDATED entries. Has no effect outside incremental mode. |
| `notificationLimit` | integer | `5` | Maximum number of entries included in each notification message (1–20). |
| `telegramToken` | string | — | Telegram bot token (from @BotFather). Required for Telegram notifications. |
| `telegramChatId` | string | — | Telegram chat or channel ID (e.g. "-100123456789"). Required when telegramToken is set. |
| `discordWebhookUrl` | string | — | Discord incoming webhook URL. Server Settings → Integrations → Webhooks → New Webhook. |
| `slackWebhookUrl` | string | — | Slack incoming webhook URL. api.slack.com/messaging/webhooks. |
| `whatsappPhoneNumberId` | string | — | Your WhatsApp Business phone-number ID (numeric, from Meta dashboard). Required when whatsappAccessToken is set. |
| `whatsappAccessToken` | string | — | WhatsApp Cloud API permanent access token (System User token from Meta Business). Recipient must have messaged the business number within the last 24h (service-conversation window — free since Nov 2024). |
| `whatsappTo` | string | — | Recipient phone in E.164 format without + (e.g. "436641234567"). Recipient must have messaged your business number within last 24h. |
| `webhookUrl` | string | — | Receives a JSON POST with {metadata, items} after each run. Universal escape hatch for n8n / Make / Zapier / custom backends. |
| `webhookHeaders` | object | — | Optional JSON object of custom headers (e.g. {"Authorization":"Bearer ..."}). |
| `appConnector` | string | — | Optional. Pick a connected app under Settings → API & Integrations to receive your results. Best-effort across MCP connectors as Apify expands its catalog. |
| `mcpIssueTeam` | string | — | Only when the connected app is an issue tracker: the team (name or ID) the summary issue is created under, if that app requires one. |
| `descriptionFormat` | enum | `"all"` | Choose which representation of the listing description to include. `all` keeps every variant; the others keep only the selected one. |

---

## Output fields

Every product returns the same 17-field schema. Missing values are `null` — never omitted.

- `listingId`
- `drugCode`
- `genericName`
- `displayName`
- `synonyms`
- `drugClass`
- `pisPdfUrl`
- `detailUrl`
- `portalUrl`
- `sourceUrl`
- `searchQuery`
- `contentQuality`
- `detailFetched`
- `scrapedAt`
- `source`
- `contentHash`
- `changeType`

---

## Sample output

One object per product. Here is a real example from a production run:

```json
{
  "listingId": "a26b511952e61aa2695eb62bee8749b68d6073acd55b44484d607b265b45c1a3",
  "drugCode": "1011018",
  "genericName": "Paracetamol",
  "displayName": "Paracetamol",
  "synonyms": [
    "Acetaminophen"
  ],
  "drugClass": "Analgesics (Non-Opioid) & Antipyretics",
  "pisPdfUrl": "https://www.mercurydrug.com/pis/Paracetamol.pdf",
  "detailUrl": "https://www.mercurydrug.com/cgi-bin/gensearch?code=1011018",
  "portalUrl": "https://www.mercurydrug.com/cgi-bin/gensearch?code=1011018",
  "sourceUrl": "https://www.mercurydrug.com/cgi-bin/gensearch?code=1011018",
  "searchQuery": "paracetamol",
  "contentQuality": "full"
}
```

*Truncated — full records contain 17 fields. See Output fields for the complete schema.*

**[Try Mercury Drug Generic Medicine Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/mercurydrug-com-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.01 |
| Result | $0.000036 |

See the [actor on Apify](https://apify.com/blackfalcondata/mercurydrug-com-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape mercurydrug.com?**
Use this actor on Apify to extract structured data from mercurydrug.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get mercurydrug.com data as JSON, CSV, or Excel?**
The actor writes each product to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape mercurydrug.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check mercurydrug.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for ${NOUNS} extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each product gets a content hash. On subsequent runs, only new or changed products are emitted — saving time, compute, and storage. Expired products can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data

[Browse all Black Falcon Data actors →](https://apify.com/blackfalcondata?fpr=1h3gvi)

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [Mercury Drug Generic Medicine Scraper](https://apify.com/blackfalcondata/mercurydrug-com-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 10*
