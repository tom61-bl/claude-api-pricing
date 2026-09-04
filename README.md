# Claude API Pricing

A practical, source-backed guide to Claude API pricing, model costs, token rates, prompt caching, Batch API discounts, tools, limits, and operational routing.

## Contents

- [Claude API Pricing: Model Costs, Token Rates, Caching, and Batch Discounts](claude-api-pricing.md) — the editorial guide
- [Claude API cost calculator](claude-api-cost-calculator.html) — interactive HTML/JS monthly cost estimator
- [claude-api-pricing.html](claude-api-pricing.html) — deployable single-page production HTML (meta/canonical/OG/JSON-LD + embedded calculator)
- [FACTS.md](FACTS.md) — internal fact ledger (evidence type, source, check date, allowed wording, risk)

## Scope

This repository contains the Markdown and static-page source for an editorial pricing guide. Prices, model IDs, availability, limits, and feature terms are time-sensitive. The guide was checked against Anthropic's official documentation on September 4, 2026; verify live pricing before implementation. The internal fact ledger in FACTS.md records evidence type, source, check date, and risk for every time-sensitive claim.

ApiFlux is mentioned because it operates the publishing project. Its pricing, feature, and discount statements are **vendor-published claims** (one key for 100+ models, native Anthropic/OpenAI/Gemini-compatible endpoints, automatic failover, transparent per-token billing, 85% of official list price, a $1 starting credit, and zero data retention). ApiFlux's listed Claude prices and URLs (apiflux.ai, /keys, /models, /models/anthropic) were verified on September 4, 2026. The 85%-of-list and $1-credit figures are vendor positioning, not independent benchmark results; its privacy and retention terms are marked `needs_review` in FACTS.md.

## Production publishing note

A production site should render the guide at a stable URL and add a unique HTML title, meta description, canonical URL, Open Graph metadata, visible author and update information, and validated structured data (Article + FAQPage JSON-LD matching the visible FAQ). A deployable single-page HTML version is included as the source for that rendering. This GitHub repository is the source repository, not a substitute for those production SEO controls.
