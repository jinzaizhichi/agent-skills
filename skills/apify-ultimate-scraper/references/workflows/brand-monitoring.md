# Brand monitoring workflows

## Cross-platform brand mention tracking
**When:** User wants to monitor brand mentions, hashtags, or sentiment across social platforms.

### Pipeline (run each independently, combine results)
1. **Instagram mentions** -> `apify/instagram-tagged-scraper`
   - Key input: `username` (brand handle)
2. **Instagram hashtags** -> `apify/instagram-hashtag-scraper`
   - Key input: `hashtags` (branded hashtags)
3. **X/Twitter mentions** -> `apidojo/tweet-scraper`
   - Key input: `searchTerms` (brand name, handle, hashtags)
4. **Reddit mentions** -> `trudax/reddit-scraper-lite`
   - Key input: `searchQuery` (brand name)

### Output fields
Instagram: `caption`, `likesCount`, `commentsCount`, `timestamp`, `ownerUsername`
X/Twitter: `text`, `retweetCount`, `likeCount`, `replyCount`, `createdAt`, `author`
Reddit: `title`, `body`, `score`, `numComments`, `subreddit`, `createdAt`

### Gotcha
This is a parallel workflow, not sequential. Run each Actor independently. Combine results by date for a timeline view.

## Sentiment analysis
**When:** User wants sentiment scoring on collected mentions.

### Pipeline
1. **Collect mentions** (use any step from above)
2. **Analyze sentiment** -> `tri_angle/social-media-sentiment-analysis-tool`
   - Pipe: collected post URLs -> `urls`
   - Key input: `urls`, `platforms`

### Output fields
Step 2: `sentiment` (positive/negative/neutral), `score`, `text`, `platform`
