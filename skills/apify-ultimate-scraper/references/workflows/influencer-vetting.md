# Influencer vetting workflows

## Instagram creator vetting
**When:** User wants to evaluate an influencer's profile, audience, and engagement quality.

### Pipeline
1. **Get profile data** -> `apify/instagram-profile-scraper`
   - Key input: `usernames` (list of handles)
2. **Analyze engagement** -> `apify/instagram-comment-scraper`
   - Pipe: `results[].latestPosts[].url` -> `directUrls` (pick 3-5 recent posts)
   - Key input: `directUrls`, `resultsLimit`

### Output fields
Step 1: `username`, `fullName`, `followersCount`, `followsCount`, `postsCount`, `biography`, `isVerified`, `latestPosts[]`
Step 2: `text`, `ownerUsername`, `timestamp` (scan for bot patterns: generic praise, emoji-only, irrelevant content)

### Gotcha
High follower count with low comment quality suggests fake followers. Compare comment sentiment to post content.

## Cross-platform influencer discovery
**When:** User wants to find an influencer's presence across multiple platforms.

### Pipeline
1. **Search across platforms** -> `tri_angle/social-media-finder`
   - Key input: `query` (influencer name or handle), `platforms`

### Output fields
Step 1: `platform`, `profileUrl`, `username`, `followers`, `isVerified`
