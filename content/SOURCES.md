# SOURCES.md — Claude API Pricing Source Ledger

Public source ledger for the guide. Every time-sensitive claim below is tied to an official or vendor source and a check date.

## Pricing & model facts

| Claim | Source | Checked | Allowed wording |
|---|---|---|---|
| Claude Haiku 4.5 $1 / $5 per MTok (in/out) | Anthropic pricing | 2026-09-04 | "Anthropic lists…" |
| Claude Sonnet 5 $2 / $10 per MTok (in/out), now standard | Anthropic pricing | 2026-09-04 | "Anthropic lists… now standard pricing" |
| Claude Opus 5 $5 / $25 per MTok (in/out) | Anthropic pricing | 2026-09-04 | "Anthropic lists…" |
| Claude Fable 5.1 $10 / $50 per MTok (in/out) | Anthropic pricing / models overview | 2026-09-04 | "Anthropic lists…" |
| Sonnet 5 $2/$10 is standard (Sept 1, 2026 increase canceled) | Anthropic pricing note | 2026-09-04 | "the scheduled increase was canceled; $2/$10 is now standard" |
| Fable 5.1 cache hits/refreshes at $0.25 / MTok = 0.025× | Anthropic pricing (model-specific table) | 2026-09-04 | "Anthropic's current pricing table lists Fable 5.1 cache hits at $0.25/MTok (0.025×). Recheck before publication." |
| Standard cache multipliers 1.25× / 2× / 0.1× (5m write / 1h write / read) | Anthropic prompt caching | 2026-09-04 | "Anthropic's documentation lists…" |
| Batch API = 50% off standard input/output tokens | Anthropic Message Batches API | 2026-09-04 | "Anthropic documents a 50% discount…" |
| Batch limits: 100,000 requests or 256 MB; ≤24 h processing; results 29 days | Anthropic Message Batches API | 2026-09-04 | "a batch is limited to…" |
| Web search $10 / 1,000 searches; web fetch no extra charge | Anthropic pricing (tool use) | 2026-09-04 | "Anthropic's pricing documentation lists web search at…" |
| Spend caps: Start $500 / Build $1,000 / Scale $200,000 monthly | Anthropic rate limits | 2026-09-04 | "Anthropic's rate-limits documentation says…" |
| Model IDs: claude-fable-5-1, claude-opus-5, claude-sonnet-5, claude-haiku-4-5-20251001 | Anthropic models overview | 2026-09-04 | publish as API ID |
| US-only inference (`inference_geo:"us"`) = 1.1× for 4.6+ models | Anthropic pricing (data residency) | 2026-09-04 | "a 1.1× multiplier" |

## ApiFlux facts (vendor claims)

| Claim | Source | Checked | Allowed wording |
|---|---|---|---|
| One API key for 100+ frontier models | ApiFlux homepage | 2026-09-04 | "ApiFlux advertises…" |
| Native Anthropic / OpenAI / Gemini-compatible APIs | ApiFlux homepage | 2026-09-04 | "advertises native … compatible endpoints" |
| Automatic failover | ApiFlux homepage | 2026-09-04 | "advertises automatic failover" |
| Transparent per-token billing, one shared balance, usage monitoring | ApiFlux homepage | 2026-09-04 | "advertises transparent per-token billing…" |
| Pricing at 85% of official list (15% below) | ApiFlux homepage / models | 2026-09-04 | "ApiFlux advertises pricing at 85% of the maker's official list price" |
| $1 starting credit, no credit card required | ApiFlux homepage | 2026-09-04 | "currently advertises a $1 starting credit" |
| Zero data retention | ApiFlux homepage | 2026-09-04 | "ApiFlux describes zero data retention in its public materials. Review the current privacy and retention terms before sending sensitive prompts." |
| Claude prices on ApiFlux: Fable 5 $8.50/$42.50; Opus 5 $4.25/$21.25; Sonnet 5 $1.70/$8.50; Haiku 4.5 $0.85/$4.25 | ApiFlux /models/anthropic | 2026-09-04 | "ApiFlux's listed Claude prices…" |

## Update triggers

Re-check immediately when: Anthropic changes rates, model IDs, caching multipliers, Batch terms, tool prices, spend limits, or lifecycle dates; ApiFlux changes prices, supported models, privacy/retention terms, or page URLs. Check model IDs and lifecycle monthly. Review FAQ and structure quarterly.
