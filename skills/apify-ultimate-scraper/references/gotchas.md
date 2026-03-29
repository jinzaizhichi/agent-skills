# Gotchas and cost guardrails

## Pricing models

| Model | How it works | Action before running |
|-------|-------------|----------------------|
| FREE | No per-result cost, only platform compute | None needed |
| PAY_PER_EVENT (PPE) | Charged per result item | MUST estimate cost first |
| FLAT_PRICE_PER_MONTH | Monthly subscription | Verify user has active subscription |

To check an Actor's pricing:

    apify actors info "ACTOR_ID" --json

Read `.currentPricingInfo.pricingModel` and `.currentPricingInfo.pricePerEvent`.

## Cost estimation protocol

Before running any PPE Actor:

1. Get the per-event price from Actor info (`.currentPricingInfo.pricePerEvent`)
2. Multiply by the requested result count
3. Present the estimate to the user: "This will cost approximately $X for Y results"
4. If estimate > $5: warn explicitly
5. If estimate > $20: require explicit user confirmation before proceeding

## Common pitfalls

**Cookie-dependent Actors**
Some social media scrapers require cookies or login sessions. If an Actor returns auth errors or empty results unexpectedly, check its README:

    apify actors info "ACTOR_ID" --readme

Look for mentions of "cookies", "login", "session", or "proxy".

**Rate limiting on large scrapes**
Platforms throttle or block large-volume scraping. Mitigations:
- Use proxy configuration when available: `"proxyConfiguration": {"useApifyProxy": true}`
- Set reasonable concurrency limits (check the Actor's `maxConcurrency` input)
- For 1,000+ results, suggest splitting into smaller batches

**Empty results**
Common causes:
- Too-narrow search query or geo-restriction (try broader terms)
- Platform blocking without proxy (enable Apify Proxy)
- Actor requires cookies/login but none provided
- Wrong input field name (always verify with `--input --json`)

**maxResults vs maxCrawledPages**
Different Actors use different limit field names. Common variants:
- `maxResults`, `resultsLimit`, `maxItems` - limit output items
- `maxCrawledPages`, `maxRequestsPerCrawl` - limit pages visited
Always fetch the input schema to find the correct field for the specific Actor.

**Deprecated Actors**
Check `.isDeprecated` in `apify actors info --json`. If `true`:
1. Search for alternatives: `apify actors search "SIMILAR_KEYWORDS" --json`
2. Prefer `apify` tier replacements over `community` alternatives

**LinkedIn pricing**
LinkedIn Actors are all PPE and vary significantly:
- `harvestapi/` Actors: generally cheaper ($0.001-0.01/result)
- `apimaestro/` Actors: generally more expensive ($0.005-0.02/result)
- `dev_fusion/` Actors: mid-range, useful for mass scraping with email enrichment
Always compare pricing before selecting a LinkedIn Actor.

**SEO tool pricing**
`radeance/` SEO scrapers (SimilarWeb, Ahrefs, SEMrush, Moz) have the highest per-result costs ($0.005-0.0275/result). For large-scale SEO analysis, estimate costs carefully and suggest batching.
