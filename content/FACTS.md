# FACTS.md — Claude API Pricing Fact Ledger

Internal evidence ledger for the guide. This file is for auditability; not all rows are published verbatim in the article.

## Pricing & model facts

| Claim | Evidence type | Source | Checked | Allowed wording | Risk |
|---|---|---|---|---|---|
| Claude Haiku 4.5 $1 / $5 per MTok (in/out) | Official source | Anthropic pricing | 2026-09-04 | "Anthropic lists…" | Low |
| Claude Sonnet 5 $2 / $10 per MTok (in/out), now standard | Official source | Anthropic pricing | 2026-09-04 | "Anthropic lists… now standard pricing" | Medium |
| Claude Opus 5 $5 / $25 per MTok (in/out) | Official source | Anthropic pricing | 2026-09-04 | "Anthropic lists…" | Low |
| Claude Fable 5.1 $10 / $50 per MTok (in/out) | Official source | Anthropic pricing / models overview | 2026-09-04 | "Anthropic lists…" | Low |
| Sonnet 5 $2/$10 is standard (Sept 1, 2026 increase canceled) | Official source | Anthropic pricing note + news | 2026-09-04 | "the scheduled increase was canceled; $2/$10 is now standard" | Medium |
| Fable 5.1 cache hits/refreshes at $0.25 / MTok = 0.025× | Official source (per editorial directive) | Anthropic pricing (model-specific table) | 2026-09-04 | "Anthropic's current pricing table lists Fable 5.1 cache hits at $0.25/MTok (0.025×)" | High — model-specific, recheck before publication |
| Standard cache multipliers 1.25× / 2× / 0.1× (5m write / 1h write / read) | Official source | Anthropic prompt caching | 2026-09-04 | "Anthropic's documentation lists…" | Low |
| Batch API = 50% off standard input/output tokens | Official source | Anthropic Message Batches API | 2026-09-04 | "Anthropic documents a 50% discount…" | Low |
| Batch limits: 100,000 requests or 256 MB; ≤24 h processing; results 29 days | Official source | Anthropic Message Batches API | 2026-09-04 | "a batch is limited to…" | Low |
| Web search $10 / 1,000 searches; web fetch no extra charge | Official source | Anthropic pricing (tool use) | 2026-09-04 | "Anthropic's pricing documentation lists web search at…" | Medium |
| Spend caps: Start $500 / Build $1,000 / Scale $200,000 monthly | Official source | Anthropic rate limits | 2026-09-04 | "Anthropic's rate-limits documentation says…" | Low |
| Model IDs: claude-fable-5-1, claude-opus-5, claude-sonnet-5, claude-haiku-4-5-20251001 | Official source | Anthropic models overview | 2026-09-04 | publish as API ID | Medium — verify aliases per platform |
| US-only inference (`inference_geo:"us"`) = 1.1× for 4.6+ models | Official source | Anthropic pricing (data residency) | 2026-09-04 | "a 1.1× multiplier" | Low |

## ApiFlux facts (vendor claims)

| Claim | Evidence type | Source | Checked | Allowed wording | Risk |
|---|---|---|---|---|---|
| One API key for 100+ frontier models | Vendor claim | ApiFlux homepage | 2026-09-04 | "ApiFlux advertises…" | Medium |
| Native Anthropic / OpenAI / Gemini-compatible APIs | Vendor claim | ApiFlux homepage | 2026-09-04 | "advertises native … compatible endpoints" | Medium |
| Automatic failover | Vendor claim | ApiFlux homepage | 2026-09-04 | "advertises automatic failover" | Medium — test with workload |
| Transparent per-token billing, one shared balance, usage monitoring | Vendor claim | ApiFlux homepage | 2026-09-04 | "advertises transparent per-token billing…" | Medium |
| Pricing at 85% of official list (15% below) | Vendor claim | ApiFlux homepage / models | 2026-09-04 | "ApiFlux advertises pricing at 85% of the maker's official list price" | High |
| $1 starting credit, no credit card required | Vendor claim | ApiFlux homepage | 2026-09-04 | "currently advertises a $1 starting credit" | High |
| Zero data retention | Vendor claim | ApiFlux homepage | 2026-09-04 | "ApiFlux also describes zero data retention; review the current privacy and retention terms" | High — privacy status needs_review |
| Claude prices on ApiFlux: Fable 5 $8.50/$42.50; Opus 5 $4.25/$21.25; Sonnet 5 $1.70/$8.50; Haiku 4.5 $0.85/$4.25 | Vendor-published price | ApiFlux /models/anthropic | 2026-09-04 | "ApiFlux's listed Claude prices…" | High — vendor pricing, not benchmark |

## Planning data (do NOT publish as verified)

| Claim | Evidence type | Source | Checked | Allowed wording | Risk |
|---|---|---|---|---|---|
| "claude api pricing" ~6,600 monthly searches, KD 14 | User-supplied planning data | Keyword tool not supplied | Pending | Do not publish as verified fact; keep internal | High |
| Monthly cost examples ($12,500 / $8,000 / $20,000) | Editorial calculation | Derived from official rates | 2026-09-04 | "illustrative figures for method demonstration only" | Medium |

## Update triggers

Re-check immediately when: Anthropic changes rates, model IDs, caching multipliers, Batch terms, tool prices, spend limits, or lifecycle dates; ApiFlux changes prices, supported models, privacy/retention terms, or page URLs. Check model IDs and lifecycle monthly. Review FAQ and structure quarterly. Track Search Console queries, CTR, ranking, and conversion data after launch.
