# Contributing

Thanks for your interest in improving this guide. This repository is the source for the Claude API pricing editorial guide published on the ApiFlux blog.

## How to contribute

1. **Fork and branch** — fork the repo, create a feature branch from `main`.
2. **Edit the source** — the article lives in `content/claude-api-pricing.md`. The deployable single-page HTML is `content/claude-api-pricing.html`; the interactive calculator is `content/claude-api-cost-calculator.html`.
3. **Verify facts** — every price, model ID, limit, and feature claim must cite an official source. Record time-sensitive claims in `content/FACTS.md` with evidence type, source URL, check date, allowed wording, and risk level.
4. **Keep vendor claims labeled** — ApiFlux pricing, features, and discounts are vendor-published claims. Use wording such as "ApiFlux advertises…" and do not present them as independent benchmarks.
5. **Open a PR** — describe what changed and why. If you updated a price, include the source URL and check date.

## Content standards

- Prices are quoted per MTok (one million tokens), input and output separately.
- Do not claim a permanent free tier for the Anthropic API unless the current official billing documentation establishes one.
- Prompt caching and Batch API modifiers are model- and platform-specific; calculate each token category separately.
- Do not invent savings, rankings, or benchmarks.
- Images live in `assets/` and are referenced from `content/` with relative paths (`../assets/…`).

## Before merging

- Recheck Anthropic's live pricing and model documentation.
- Confirm all relative links and image paths resolve.
- Update the changelog in `content/claude-api-pricing.md` and the fact ledger in `content/FACTS.md`.
