# Review analysis workflows

## Google Maps review extraction
**When:** User wants to collect and analyze business reviews from Google Maps.

### Pipeline
1. **Find businesses** -> `compass/crawler-google-places`
   - Key input: `searchStringsArray`, `locationQuery`, `maxCrawledPlaces`
2. **Extract reviews** -> `compass/Google-Maps-Reviews-Scraper`
   - Pipe: `results[].url` -> `startUrls`
   - Key input: `startUrls`, `maxReviews`

### Output fields
Step 1: `title`, `totalScore`, `reviewsCount`, `url`, `categoryName`
Step 2: `text`, `stars`, `publishedAtDate`, `reviewerName`, `ownerResponse`

## Cross-platform hotel/restaurant reviews
**When:** User wants reviews aggregated from multiple platforms for the same business.

### Pipeline (hotels)
1. **Aggregate reviews** -> `tri_angle/hotel-review-aggregator`
   - Key input: `urls` (hotel URLs from TripAdvisor, Yelp, Google Maps, Booking.com, etc.)

### Pipeline (restaurants)
1. **Aggregate reviews** -> `tri_angle/restaurant-review-aggregator`
   - Key input: `urls` (restaurant URLs from Yelp, Google Maps, DoorDash, UberEats, etc.)

### Output fields
Both: `text`, `rating`, `date`, `platform`, `reviewerName`, `title`

## Yelp review pipeline
**When:** User wants Yelp reviews for businesses in a specific area.

### Pipeline
1. **Find businesses** -> `tri_angle/get-yelp-urls`
   - Key input: `location`, `category`
2. **Extract reviews** -> `tri_angle/yelp-review-scraper`
   - Pipe: `results[].url` -> `startUrls`
   - Key input: `startUrls`, `maxReviews`

### Output fields
Step 1: `name`, `url`, `rating`, `reviewCount`, `address`
Step 2: `text`, `rating`, `date`, `userName`

### Gotcha
Review aggregators pull from multiple platforms in one run - cheaper than running separate scrapers per platform. Use the aggregators when covering 3+ platforms.
