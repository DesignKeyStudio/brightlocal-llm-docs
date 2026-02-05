# Offsite SEO API

> **Status**: Current

## Overview

The Offsite SEO API returns domain-level SEO metrics for a given website, including domain age, hosting location, number of indexed pages, and domain authority.

This is a **Batch Method** API - requests must be submitted within a batch container.

## Authentication

- API key required via `api-key` parameter
- Base URL: `https://tools.brightlocal.com/seo-tools/api`

## Endpoints

### Fetch Domain Metrics

**`POST /v4/seo/offsite`**

Retrieves offsite SEO metrics for a specified domain.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| batch-id | integer | Yes | Batch container ID |
| website-url | string | Yes | URL of the business website (with or without http/https) |

#### Request Example

```bash
curl -X POST "https://tools.brightlocal.com/seo-tools/api/v4/seo/offsite" \
  -d "api-key=YOUR_API_KEY" \
  -d "batch-id=17" \
  -d "website-url=http://www.example.com"
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
    "domain-age": "15 years",
    "hosting-country": "United States",
    "indexed-pages": 1250,
    "domain-authority": 45,
    "backlinks": 3200,
    "referring-domains": 180
  }
}
```

## Response Fields

| Field | Type | Description |
|-------|------|-------------|
| domain-age | string | Age of the domain |
| hosting-country | string | Country where domain is hosted |
| indexed-pages | integer | Number of pages indexed in search engines |
| domain-authority | integer | Domain authority score (0-100) |
| backlinks | integer | Total number of backlinks |
| referring-domains | integer | Number of unique referring domains |

## Error Codes

| Error Code | HTTP Status | Description |
|------------|-------------|-------------|
| INVALID_API_KEY | 401 | API key is missing or invalid |
| INVALID_BATCH_ID | 400 | Batch ID not found |
| INVALID_WEBSITE_URL | 400 | Website URL is missing or invalid |
| INVALID_METHOD | 405 | Only POST method is allowed |

#### Error Response Example

```json
{
  "success": false,
  "errors": {
    "INVALID_WEBSITE_URL": "Website URL is missing"
  }
}
```

## Usage with Batches

```bash
# 1. Create batch
curl -X POST "https://tools.brightlocal.com/seo-tools/api/v4/batch" \
  -d "api-key=YOUR_API_KEY"
# Returns: {"batch-id": "17"}

# 2. Add offsite SEO job
curl -X POST "https://tools.brightlocal.com/seo-tools/api/v4/seo/offsite" \
  -d "api-key=YOUR_API_KEY" \
  -d "batch-id=17" \
  -d "website-url=http://www.example.com"

# 3. Commit batch
curl -X PUT "https://tools.brightlocal.com/seo-tools/api/v4/batch" \
  -d "api-key=YOUR_API_KEY" \
  -d "batch-id=17"

# 4. Poll for results
curl -X GET "https://tools.brightlocal.com/seo-tools/api/v4/batch?api-key=YOUR_API_KEY&batch-id=17"
```

## Notes

- Results are returned asynchronously; poll the batch endpoint for completion
- URL can be specified with or without the http/https protocol prefix
- Domain authority scores use a 0-100 scale
- This API is part of the older API suite but remains current
