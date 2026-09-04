# Claude API Pricing

A practical, source-backed guide to Claude API pricing, model costs, token rates, prompt caching, Batch API discounts, tools, limits, and operational routing.

## Repository structure

```
.
├── README.md
├── LICENSE                  # MIT
├── CONTRIBUTING.md
├── .gitignore
├── assets/                  # cover and section visual assets
│   ├── cover-hero-1600x900.png
│   ├── cover-og-1200x630.png
│   ├── cover-listing-640x360.png
│   ├── cover-thumbnail-320x180.png
│   ├── section-decision-router-1280x720.png
│   ├── section-cache-breakeven-1280x720.png
│   └── section-monthly-cost-1280x720.png
├── content/                 # article source and deployable pages
│   ├── claude-api-pricing.md
│   ├── claude-api-cost-calculator.html
│   ├── claude-api-pricing.html
│   └── SOURCES.md
```

## Contents

- [Claude API Pricing: Model Costs, Token Rates, Caching, and Batch Discounts](content/claude-api-pricing.md) — the editorial guide
- [Claude API cost calculator](content/claude-api-cost-calculator.html) — interactive HTML/JS monthly cost estimator
- [claude-api-pricing.html](content/claude-api-pricing.html) — deployable single-page production HTML (meta/canonical/OG/JSON-LD + embedded calculator)
- [SOURCES.md](content/SOURCES.md) — public source ledger (claim, source, check date, allowed wording)

## Scope

This repository contains the Markdown and static-page source for an editorial pricing guide. Prices, model IDs, availability, limits, and feature terms are time-sensitive. The guide was checked against Anthropic's official documentation on September 4, 2026; verify live pricing before implementation. The public source ledger in `content/SOURCES.md` records claim, source, check date, and allowed wording for every time-sensitive claim.

ApiFlux is mentioned because it operates the publishing project. Its pricing, feature, and discount statements are **vendor-published claims** (one key for 100+ models, native Anthropic/OpenAI/Gemini-compatible endpoints, automatic failover, transparent per-token billing, 85% of official list price, a $1 starting credit, and zero data retention). ApiFlux's listed Claude prices and URLs (apiflux.ai, /keys, /models, /models/anthropic) were verified on September 4, 2026. The 85%-of-list and $1-credit figures are vendor positioning, not independent benchmark results; review its current privacy and retention terms before relying on the zero-data-retention claim.

## Production publishing note

A production site should render the guide at a stable URL and add a unique HTML title, meta description, canonical URL, Open Graph metadata, visible author and update information, and validated structured data (Article + FAQPage JSON-LD matching the visible FAQ). A deployable single-page HTML version is included as the source for that rendering. This GitHub repository is the source repository, not a substitute for those production SEO controls.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to submit changes, verify facts, and label vendor claims.

## License

Released under the [MIT License](LICENSE).
