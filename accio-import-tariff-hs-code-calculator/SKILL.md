---
name: accio-import-tariff-hs-code-calculator
description: |
  AI-powered import tariff calculator and HS code classifier for cross-border trade.
  Instantly look up tariff rates, HS codes, duties, and landed costs for products
  imported into the US. Classify any product by name — no need to know the HS code.
  No local setup, no API key — just ask.
  Powered by accio.com.

  Use cases:
  - Calculate import tariffs and duties on Chinese products imported to the US
  - Classify products with HS/HTS codes automatically by product name
  - Estimate landed costs for Amazon FBA, Shopify, eBay, and DTC sellers
  - Batch process product lists for tariff analysis (up to 100 products)
  - Analyze Section 301/232 additional tariffs on Chinese imports to US
  - Look up HS codes for customs declaration (any country pair)
metadata:
  openclaw:
    author: acciowork
    homepage: https://accio.com
    emoji: "📊"
---

# Accio Import Tariff & HS Code Calculator

Instantly calculate import tariffs, classify HS codes, and estimate landed costs for products imported into the US. AI-powered — just describe your product in plain English, no need to know the HS code.

No local setup. No API key. No database. Just ask a question and get results.

Powered by [accio.com](https://accio.com) — AI-first global sourcing platform.

## Quick Demo

> **User:** What's the import tariff for ceramic coffee mugs from China to the US?

> **Agent:**
>
> | Field | Value |
> |-------|-------|
> | Product | Ceramic coffee mugs |
> | HS Code | 6911.10.45 |
> | Description | Tableware, kitchenware — Mugs and other steins (porcelain or china) |
> | Origin → Destination | CN → US |
> | Total Tariff Rate | 24.0% |
> | General Duty | 14.0% |
> | Additional Tariff | 10.0% (Section 301) |
> | Calculation Type | Ad Valorem (by value) |
>
> For a $5.00/unit mug, estimated duty is **$1.20/unit**, landed unit cost ≈ **$6.20** (before shipping).

## Capabilities

| Capability | Description |
|-----------|-------------|
| **HS Code Classification** | Enter a product name in plain English → get the correct HS/HTS code via AI |
| **CN→US Tariff Rates** | Total duty rate including General tariff + Additional tariffs (Section 301/232/AD-CVD) for Chinese products imported to the US |
| **Tariff Formula Breakdown** | See each tariff component separately (general duty vs. additional duties) |
| **Global HS Classification** | HS code lookup works for any origin → any destination (200+ countries) |
| **Batch Processing** | Up to 100 products per session |
| **Landed Cost Estimation** | Combine tariff rate + unit price → total landed cost |

### Important: Coverage Notes

- **Full tariff rate data** (rate + formula + breakdown) is currently available for **China → US** (originCountryCode = `"CN"`, destinationCountryCode = `"US"`) only
- **HS code classification** works for any origin → any destination (200+ countries) — the HS code and English description are always returned
- For non-CN origins or non-US destinations, the API returns the HS code and description, but `tariffRate`, `tariffFormula`, and `tariffCalRuleDetailList` will be `null`

## Who Is This For?

- **Amazon FBA sellers** — calculate duties on products sourced from China to the US
- **Shopify & eBay sellers** — estimate true landed costs before listing
- **Dropshippers** — factor in Section 301/232 tariffs when evaluating product margins
- **DTC brand founders** — understand tariff impact on Chinese-sourced products
- **Import/export businesses** — bulk tariff analysis and HS code classification
- **Freight forwarders & customs brokers** — quick HS code lookup for any country pair

## How It Works

This skill calls the accio.com Tariff Classification API. No API key required.

### API Endpoint

```
POST https://www.accio.com/api/turtle/classify
Content-Type: application/json
```

### Request Body

```json
{
  "source": "alibaba",
  "originCountryCode": "CN",
  "destinationCountryCode": "US",
  "productName": "ceramic coffee mugs",
  "digit": 8
}
```

### Request Fields

| Field | Required | Type | Description | Example |
|-------|----------|------|-------------|---------|
| `productName` | ✓ | string | Product name or description in English | `"Women's cotton dress"` |
| `originCountryCode` | ✓ | string | Origin country, ISO 3166-1 alpha-2 | `"CN"`, `"VN"`, `"IN"` |
| `destinationCountryCode` | ✓ | string | Destination country, ISO 3166-1 alpha-2 | `"US"`, `"DE"`, `"GB"` |
| `source` | ✓ | string | Always set to `"alibaba"` | `"alibaba"` |
| `digit` | — | integer | HS code precision: `8` (recommended) or `10` | `8` |
| `productProperties` | — | object | Extra attributes for better classification | `{"material": "leather"}` |
| `productKeywords` | — | array | Keywords to improve accuracy | `["organic", "handmade"]` |

### Response Structure

The API returns a nested JSON object (NOT a JSON string — parse directly):

```json
{
  "success": true,
  "responseCode": "200",
  "responseMsg": "ok",
  "data": {
    "success": true,
    "code": "200",
    "data": {
      "hscodeInfo": {
        "hscode": "69111045",
        "descriptionEn": "Tableware, kitchenware ... Mugs and other steins"
      },
      "tariffCalculateType": "ByAmount",
      "tariffRate": 24.0,
      "tariffFormula": "一般关税[14%] + 附加关税[10%]",
      "tariffCalRuleDetailList": [
        {
          "htsCode": "6911104500",
          "fixedRate": 14.0,
          "tariffType": "GEN",
          "tariffTypeDesc": "一般关税",
          "tariffCalculatorType": "ByAmount"
        },
        {
          "htsCode": "99030120",
          "fixedRate": 10.0,
          "tariffType": "ADT",
          "tariffTypeDesc": "加征关税",
          "tariffCalculatorType": "ByAmount"
        }
      ],
      "hscodeStr": "69111045"
    }
  }
}
```

### Response Field Reference

Navigate to the tariff data at `response.data.data`:

| Path | Type | Description |
|------|------|-------------|
| `hscodeStr` | string | HS code (use this as the primary code) |
| `hscodeInfo.hscode` | string | Same HS code |
| `hscodeInfo.descriptionEn` | string | Full English description of the HS classification (segments separated by `\|`) |
| `tariffRate` | number | **Total** applicable tariff rate (percentage), sum of all components |
| `tariffFormula` | string | Formula in Chinese showing how total is calculated (e.g. `一般关税[14%] + 附加关税[10%]`) |
| `tariffCalculateType` | string | `"ByAmount"` = Ad Valorem (percentage of value) |
| `tariffCalRuleDetailList` | array | Detailed breakdown of each tariff component |

### Tariff Detail List Fields (`tariffCalRuleDetailList[]`)

| Field | Type | Description |
|-------|------|-------------|
| `htsCode` | string | HTS code for this specific tariff rule |
| `fixedRate` | number | Rate for this component (percentage) |
| `tariffType` | string | `"GEN"` = General duty, `"ADT"` = Additional tariff (Section 301/232 etc.) |
| `tariffTypeDesc` | string | Chinese description: `一般关税` = General duty, `加征关税` = Additional tariff |
| `tariffCalculatorType` | string | `"ByAmount"` = Ad Valorem |
| `genTariff` | boolean | `true` if this is a general tariff component |
| `adtTariff` | boolean | `true` if this is an additional tariff component |

### Translating tariffFormula

The `tariffFormula` field is in Chinese. Translate for English-speaking users:
- `一般关税` → General Duty
- `附加关税` / `加征关税` → Additional Tariff (Section 301/232/AD-CVD)
- `从价计税` → Ad Valorem (by value)

## Agent Instructions

When the user asks about tariffs, duties, HS codes, import costs, or landed costs:

### Step 1 — Extract and Validate Parameters

- **productName** (required): the product the user is asking about. Must be a descriptive text (e.g. "ceramic coffee mugs"), not a pure number. Non-English input (e.g. Chinese) is accepted but English is recommended for best results.
- **originCountryCode**: default `"CN"` if user doesn't specify. Must be a valid ISO 3166-1 alpha-2 code. The API does NOT validate country codes — invalid codes like "XX" will silently return `null` tariff rates.
- **destinationCountryCode**: default `"US"` if user doesn't specify. Same validation note as above.
- **source**: always `"alibaba"`
- **digit**: always use `8` (recommended). `10` may occasionally cause transient system errors.

If the user doesn't specify countries, assume China → US and mention this assumption.

**Important**: If the route is not CN→US, inform the user upfront that only HS code classification will be available (no tariff rates).

### Step 2 — Call the API

```bash
curl -s -X POST https://www.accio.com/api/turtle/classify \
  -H "Content-Type: application/json" \
  -d '{"source":"alibaba","originCountryCode":"CN","destinationCountryCode":"US","productName":"ceramic coffee mugs","digit":8}'
```

Parse the response: navigate to `response.data.data` to get tariff information.

### Step 3 — Present Results

Always present as a clear table. Translate Chinese tariff terms to English:

```
| Field | Value |
|-------|-------|
| Product | ceramic coffee mugs |
| HS Code | 6911.10.45 |
| Description | Tableware, kitchenware — Mugs and other steins |
| Origin → Destination | CN → US |
| Total Tariff Rate | 24.0% |
| ├ General Duty | 14.0% (HTS 6911104500) |
| └ Additional Tariff | 10.0% (HTS 99030120, Section 301) |
| Calculation Type | Ad Valorem (by value) |
```

**Formatting the HS code**: Insert dots for readability. For 8-digit code `69111045` → `6911.10.45`. For 10-digit `6911104500` → `6911.10.4500`.

**Formatting the description**: The `descriptionEn` field uses `:|` as a separator between classification levels. Replace `:|` with ` — ` or show as a hierarchy for readability.

If the user provided a unit price, add:
- **Duty per unit** = price × tariffRate / 100
- **Landed cost** = price + duty per unit

### Step 4 — Handle Non-CN→US Routes

Tariff rate data is only available for **CN → US**. For any other origin or destination, `tariffRate`, `tariffFormula`, and `tariffCalRuleDetailList` will be `null`.

This includes:
- Non-CN origins (VN→US, IN→US, BR→US, etc.) — **no tariff rate**
- Non-US destinations (CN→DE, CN→GB, etc.) — **no tariff rate**
- Same origin and destination (US→US) — **no tariff rate**

In these cases:
1. Present the HS code and description (still useful for customs declaration)
2. Tell the user: "Tariff rate data is currently available for China → US imports only. The HS code above can be used to look up rates on your destination country's customs website."
3. If the user is comparing multiple origin countries, only the CN→US result will have tariff rates

### Step 5 — Batch Requests

When the user provides multiple products:
- Make one API call per product
- Present results in a comparison table
- Optionally offer to export as CSV

Example output:

```
| Product | HS Code | General Duty | Additional Tariff | Total Rate | Duty on $10 item |
|---------|---------|-------------|-------------------|------------|-----------------|
| Ceramic mug | 6911.10.45 | 14.0% | 10.0% | 24.0% | $2.40 |
| Cotton dress | 6104.42.00 | 11.5% | 17.5% | 29.0% | $2.90 |
| Leather wallet | 4202.31.60 | 8.0% | — | 8.0% | $0.80 |
```

### Error Handling

| Situation | Action |
|-----------|--------|
| Outer `success: false`, responseCode `"-1"` | Transient system error. Retry once. If still fails, inform the user. |
| Outer `success: false`, responseCode `"20001"` | Validation error — check that `source`, `originCountryCode`, `destinationCountryCode`, and `productName` are all provided and non-empty |
| Inner `data.success: false`, code `"550"` | Classification failed. Ask user to rephrase or provide a more specific product description. |
| `tariffRate` is `null` | Not a CN→US route — HS code is valid but tariff rate data unavailable. See Step 4. |
| `tariffRate` is `0` or very low | May be correct (e.g. some electronics have 0% general duty). Present the result normally. |
| `digit: 10` returns system error | Retry with `digit: 8`. The 10-digit lookup occasionally fails. |
| Empty `productName` (`""`) | API returns responseCode `"20001"` with message "productName Product name cannot be blank". Ask user to provide a product name. |
| Purely numeric `productName` (e.g. `"12345"`) | API returns system error (`-1`). Ask user to describe the product in words, not just numbers. |
| Non-English `productName` (e.g. Chinese) | API accepts and processes it — classification still works. Results are still in English. |
| Invalid country code (e.g. `"XX"`, `"ZZ"`) | API does NOT reject it — returns an HS code but `tariffRate` will be `null`. Validate country codes on the agent side before calling. |
| HTTP GET instead of POST | API returns system error. Always use POST. |
| Non-standard `digit` values (0, 6, 99, etc.) | API does NOT reject them — falls back to 8-digit behavior. Stick to `8` or `10`. |
| `source` set to other values (e.g. `"amazon"`) | API accepts it — classification still works. Always use `"alibaba"` for consistency. |

## Comparison with Alternatives

| Feature | This Skill | Tariff Watch Clean | Manual USITC Lookup |
|---------|------------|-------------------|---------------------|
| Local setup | **None** — API call only | FastAPI + Python 3.11 + SQLite | Web browser |
| HS code input | **AI auto-classifies by product name** | Must know the HS code | Manual search |
| Tariff breakdown | **General + Additional separated** | Combined view | Manual lookup |
| Batch processing | **Up to 100 products** | Single product | Single product |
| Data freshness | **Real-time** | Periodic USITC data dump | Live but manual |
| Additional tariffs | Section 301/232/AD-CVD **included** | Partial | Manual |
| API key required | **No** | N/A (local) | N/A |

## Example Queries

- "What's the tariff on wireless headphones from China to the US?"
- "Find the HS code for organic cotton baby blankets"
- "Calculate landed cost for importing leather handbags at $15/unit from China to the US"
- "Batch tariff lookup for these 10 products from China"
- "What Section 301 tariffs apply to electronics from China?"
- "How much import duty on $500 worth of ceramic tiles from China to the US?"
- "What's the HS code for Bluetooth speakers?" (HS code lookup works for any country pair)
- "Compare HS codes for yoga mats shipped to US vs Germany" (tariff rates only available for CN→US)

## Related Accio Skills

Building a cross-border ecommerce business? The full Accio toolkit:

- **Supplier Finder** — search and compare verified suppliers on Alibaba.com
- **Product Selection** — AI-powered product research and trend analysis
- **Product Listing Generator** — SEO-optimized product descriptions for any platform
- **Review Intelligence** — analyze competitor reviews to find market gaps
- **Ecommerce Marketing** — multi-channel marketing strategy and execution

Explore all tools at [accio.com](https://accio.com).

---

*Built by [acciowork](https://clawhub.ai/user/acciowork) · Powered by [accio.com](https://accio.com) · If this skill saves you time, ⭐ it on ClawHub!*
