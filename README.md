# Accio Agent Skills

Free, open-source AI agent skills for cross-border ecommerce. Built by [accio.com](https://accio.com) — the AI-first global sourcing platform.

These skills work with any [OpenClaw](https://github.com/nicepkg/openclaw)-compatible AI agent, including [Claude Code](https://docs.anthropic.com/en/docs/claude-code), [Cursor](https://cursor.sh), and [Accio Work](https://accio.com).

## Available Skills

| Skill | Description | Install |
|-------|-------------|---------|
| [**Import Tariff & HS Code Calculator**](./accio-import-tariff-hs-code-calculator) | AI-powered tariff lookup and HS code classification for CN→US imports. No API key needed. | `clawhub install accio-import-tariff-hs-code-calculator` |
| **Alibaba Supplier Finder** *(coming soon)* | Search and compare verified suppliers on Alibaba.com | — |
| **Product Selection AI** *(coming soon)* | AI-powered product research, trend analysis, and opportunity scoring | — |
| **Product Listing Generator** *(coming soon)* | SEO-optimized product descriptions for Amazon, Shopify, eBay, Etsy | — |
| **Review Intelligence** *(coming soon)* | Analyze competitor product reviews to find market gaps and selling points | — |
| **Ecommerce Marketing Suite** *(coming soon)* | Multi-channel marketing strategy and campaign planning | — |

## Quick Start

### Install via ClawHub

```bash
# Install a single skill
clawhub install accio-import-tariff-hs-code-calculator

# Or install directly from this repo (coming soon)
clawhub install acciowork/agent-skills/accio-import-tariff-hs-code-calculator
```

### Use Manually

Each skill is a single `SKILL.md` file. You can copy it directly into your AI agent's skill directory:

```bash
# Clone this repo
git clone https://github.com/acciowork/agent-skills.git

# Copy the skill you want
cp -r agent-skills/accio-import-tariff-hs-code-calculator /path/to/your/agent/skills/
```

## How These Skills Work

Each skill is a `SKILL.md` file — a structured instruction set that tells an AI agent how to perform a specific task. The agent reads the file and follows the instructions, which typically include:

- **What the skill does** and when to use it
- **API endpoints** to call (powered by [accio.com](https://accio.com))
- **How to parse** and present results
- **Error handling** for edge cases

No backend setup. No API keys. No databases. The AI agent does all the work.

## Why Accio Skills?

| | Accio Skills | Typical Agent Skills |
|---|---|---|
| **Setup** | Zero — API call only | Often requires local servers, databases, or API keys |
| **Data** | Real-time from [accio.com](https://accio.com) APIs | Static data or web scraping |
| **Intelligence** | AI-powered classification & analysis | Rule-based or keyword matching |
| **Ecommerce Focus** | Purpose-built for cross-border trade | Generic or loosely adapted |
| **Suite Integration** | Skills work together as a complete workflow | Standalone, disconnected tools |

## The Accio Ecommerce Workflow

These skills are designed to work together as a complete cross-border ecommerce workflow:

```
📊 Market Analysis      → What's trending? Where's the demand?
🔍 Product Selection    → Which products should I sell?
🏭 Supplier Finder      → Where do I source them?
💰 Tariff Calculator    → What will it cost to import?
✍️ Listing Generator    → How do I describe and list them?
📝 Review Intelligence  → What are customers saying about competitors?
📣 Marketing Suite      → How do I promote and sell?
```

Each skill can be used independently, but they're most powerful together. Results from one skill naturally feed into the next.

## About Accio

[Accio](https://accio.com) is an AI-first global sourcing platform that helps ecommerce sellers find products, compare suppliers, and grow their businesses.

- **AI-Powered Sourcing** — Find the right products and suppliers using AI
- **Cross-Border Trade Tools** — Tariff calculation, HS code lookup, landed cost estimation
- **Market Intelligence** — Trend analysis, demand signals, competitive insights
- **Free to Start** — No credit card required, generous free tier

👉 **[Try Accio Free](https://accio.com)** — Start sourcing smarter today.

## Contributing

We welcome contributions! See [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

- **Found a bug?** [Open an issue](https://github.com/acciowork/agent-skills/issues/new?template=bug_report.md)
- **Have a feature request?** [Tell us](https://github.com/acciowork/agent-skills/issues/new?template=feature_request.md)
- **Want to improve a skill?** Fork and submit a PR

## License

[MIT-0](./LICENSE) — free to use, modify, and distribute without attribution.

All skills call APIs provided by [accio.com](https://accio.com). The API is currently free to use; usage terms may change in the future. See [accio.com/terms](https://accio.com/terms) for details.

---

**[accio.com](https://accio.com)** · [Twitter](https://twitter.com/acciocom) · [Discord](https://discord.gg/accio)

*If these skills help your business, give this repo a ⭐ — it helps others discover these tools!*
