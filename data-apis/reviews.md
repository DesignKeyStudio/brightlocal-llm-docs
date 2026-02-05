# Reviews API

> **Status**: Current

## Overview

The Reviews API allows you to fetch review data from over 80 review sites including Google, Yelp, Facebook, TripAdvisor, and industry-specific platforms. You can retrieve review counts, star ratings, and full review content.

This is a **Batch Method** API - requests must be submitted within a batch container.

## Authentication

- API key required via `api-key` parameter
- Base URL: `https://tools.brightlocal.com/seo-tools/api`

## Endpoints

### Fetch Reviews by Profile URL

**`POST /v4/ld/fetch-reviews`**

Fetches reviews using a direct profile URL from a review site.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| batch-id | integer | Yes | Batch container ID |
| profile-url | string | Yes | Directory profile URL |
| country | string | Yes | Country code (currently: "USA") |
| sort | string | No | Sort order: 'rating' or 'date' (default: 'date') |
| reviews-limit | integer/string | No | Number of reviews or 'all' (default: 100) |
| date-from | string | No | Filter reviews from date (format: Y-m-d or Y-m-d H:i:s) |
| start-page | integer/string | No | Pagination: page number or token |

#### Request Example

```bash
curl -X POST "https://tools.brightlocal.com/seo-tools/api/v4/ld/fetch-reviews" \
  -d "api-key=YOUR_API_KEY" \
  -d "batch-id=17" \
  -d "profile-url=https://www.yelp.com/biz/example-business" \
  -d "country=USA" \
  -d "sort=date" \
  -d "reviews-limit=50"
```

#### Response Example (201 Created)

```json
{
  "success": true,
  "job-id": 318
}
```

#### Results (via Batch Get)

```json
{
  "success": true,
  "results": {
    "reviews": [
      {
        "rating": 4,
        "author": "John D.",
        "timestamp": "2024-07-30",
        "text": "Great service and friendly staff!",
        "id": "abc123hash"
      }
    ],
    "reviews-count": 1,
    "star-rating": 4.5
  }
}
```

---

### Fetch Reviews by Business Data

**`POST /v4/ld/fetch-reviews-by-business-data`**

Fetches reviews by searching for the business using business details.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| batch-id | integer | Yes | Batch container ID |
| business-names | string | Yes | Newline-separated list of possible business names (max 5) |
| city | string | Yes | City name |
| postcode | string | Yes | Postal/ZIP code |
| local-directory | string | Yes | Directory to search (see supported list) |
| country | string | Yes | Country code (currently: "USA") |
| street-address | string | No | Street address for better matching |
| telephone | string | No | Phone number (improves result quality) |
| sort | string | No | Sort order: 'rating' or 'date' (default: 'date') |
| reviews-limit | integer/string | No | Number of reviews or 'all' (default: 100) |
| date-from | string | No | Filter reviews from date |
| start-page | integer/string | No | Pagination token or page number |

#### Request Example

```bash
curl -X POST "https://tools.brightlocal.com/seo-tools/api/v4/ld/fetch-reviews-by-business-data" \
  -d "api-key=YOUR_API_KEY" \
  -d "batch-id=17" \
  -d "business-names=Example Restaurant\nExample Cafe" \
  -d "city=New York" \
  -d "postcode=10001" \
  -d "local-directory=yelp" \
  -d "country=USA" \
  -d "telephone=212-555-0100"
```

#### Response Example (201 Created)

```json
{
  "success": true,
  "job-id": 319
}
```

## Supported Directories & Pagination

| Directory | Reviews Per Page | Pagination Method |
|-----------|------------------|-------------------|
| google | Varies | Token-based |
| yelp | 500 | Page number |
| facebook | Varies | Token-based |
| trustpilot | 20 | Page number |
| tripadvisor | 5-10 | Page number |
| yellowpages | 20 | Page number |

**Note**: Maximum 500 reviews per request. Use `start-page` with returned tokens for additional pages.

## Review Response Fields

| Field | Type | Description |
|-------|------|-------------|
| rating | integer | Star rating (1-5) |
| author | string | Reviewer name |
| timestamp | string | Review date (Y-m-d format) |
| text | string | Full review text |
| id | string | Unique review identifier hash |
| reviews-count | integer | Total number of reviews found |
| star-rating | float | Average star rating |

## Error Codes

| Error Code | Description |
|------------|-------------|
| INVALID_API_KEY | API key is missing or invalid |
| INVALID_BATCH_ID | Batch ID not found |
| INVALID_PROFILE_URL | Profile URL is not valid |
| INVALID_COUNTRY | Invalid country specified |
| INVALID_LOCAL_DIRECTORY | Directory not supported |

## Supported Local Directories

Common directories include:
- google
- yelp
- facebook
- tripadvisor
- trustpilot
- yellowpages
- bbb (Better Business Bureau)
- foursquare
- citysearch
- And 70+ more industry-specific sites

## Notes

- Google My Business URLs may require the "Google Link & ID Generator tool" for proper formatting
- Trial API keys have limited free credits
- All responses are JSON-encoded
- Use batch polling to retrieve results after job completion
