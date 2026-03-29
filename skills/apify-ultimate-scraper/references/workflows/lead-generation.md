# Lead generation workflows

## Local business leads with email enrichment
**When:** User wants business contacts, emails, or phone numbers for businesses in a specific location.

### Pipeline
1. **Find businesses** -> `compass/crawler-google-places`
   - Key input: `searchStringsArray`, `locationQuery`, `maxCrawledPlaces`
2. **Enrich with contacts** -> `compass/enrich-google-maps-dataset-with-contacts`
   - Pipe: `results[].url` -> `startUrls` (or pass the dataset ID directly)
   - Key input: `datasetId` (from step 1), `maxRequestsPerCrawl`

### Output fields
Step 1: `title`, `address`, `phone`, `website`, `categoryName`, `totalScore`, `reviewsCount`, `url`
Step 2: `emails[]`, `phones[]`, `socialLinks`, `linkedInUrl`, `twitterUrl`

### Gotcha
Google Maps results vary by language and location. Set `language: "en"` explicitly. Also set `locationQuery` to a specific city/region, not just a country.

## B2B prospect discovery via LinkedIn
**When:** User wants to find professionals by role, company, or industry.

### Pipeline
1. **Search profiles** -> `harvestapi/linkedin-profile-search`
   - Key input: `keyword`, `location`, `title`, `limit`
2. **Enrich with details** -> `harvestapi/linkedin-profile-scraper`
   - Pipe: `results[].profileUrl` -> `urls`
   - Key input: `urls`, `includeEmail` (set to `true` for email discovery)

### Output fields
Step 1: `fullName`, `headline`, `location`, `profileUrl`, `currentCompany`
Step 2: `experience[]`, `education[]`, `skills[]`, `email`, `phone`

### Gotcha
LinkedIn Actors are all PPE. Step 2 with `includeEmail: true` costs ~$0.01/profile. For 500 profiles, that's ~$5. Estimate and confirm with user.
