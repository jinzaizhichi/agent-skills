# Competitive intelligence workflows

## Competitor ad monitoring
**When:** User wants to see competitor advertising creatives, targeting, or ad spend signals.

### Pipeline
1. **Scrape ad library** -> `apify/facebook-ads-scraper`
   - Key input: `searchQuery` (competitor name), `country`, `adType`, `maxItems`

### Output fields
Step 1: `adTitle`, `adBody`, `adCreativeUrl`, `startDate`, `pageInfo.name`, `platform`

### Gotcha
Facebook Ad Library is public data, no auth needed. But results are limited to currently active or recently inactive ads.

## Competitor web presence analysis
**When:** User wants traffic, rankings, and SEO data for competitor domains.

### Pipeline
1. **Get traffic data** -> `radeance/similarweb-scraper`
   - Key input: `urls` (competitor domains)
2. **Get backlink profile** -> `radeance/ahrefs-scraper`
   - Key input: `urls` (same domains)

### Output fields
Step 1: `globalRank`, `monthlyVisits`, `bounceRate`, `avgVisitDuration`, `trafficSources`
Step 2: `domainRating`, `backlinks`, `referringDomains`, `organicKeywords`

### Gotcha
SEO scrapers (radeance/) have the highest PPE costs ($0.005-0.0275/result). Estimate cost before running. For a single domain, each step costs ~$0.02-0.03.
