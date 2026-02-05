# BrightLocal LLM Docs

Comprehensive API documentation for BrightLocal's Local SEO Tools, optimized for LLM consumption.

## API Portals

BrightLocal maintains two API documentation portals:

| Portal | Base URL | Description |
|--------|----------|-------------|
| **Legacy Portal** | `https://tools.brightlocal.com/seo-tools/api` | Older APIs (Batches, Reviews, Citation Tracker) |
| **Developer Portal** | `https://api.brightlocal.com` | Newer APIs (Rankings, Listings, Clients, Locations) |

## Authentication

### Legacy Portal (apidocs.brightlocal.com)

Include API key as a parameter:
```bash
curl -X POST "https://tools.brightlocal.com/seo-tools/api/v4/batch?api-key=YOUR_API_KEY"
```

### Developer Portal (developer.brightlocal.com)

Include API key in the `x-api-key` header:
```bash
curl --header 'x-api-key: YOUR_API_KEY' https://api.brightlocal.com/...
```

## Documentation Structure

### Core Documentation

| Document | Description |
|----------|-------------|
| [Getting Started](core/getting-started.md) | Authentication, base URL, API types, quick start guide |
| [Batches API](core/batches.md) | Batch processing for asynchronous API requests |
| [Reference](core/reference.md) | Supported countries, directories, error codes |

### Data APIs - Developer Portal (api.brightlocal.com)

These are the newer APIs from the Developer Portal using async request/callback pattern.

| Document | Endpoint | Description |
|----------|----------|-------------|
| [Rankings API](data-apis/rankings.md) | `/data/v1/rankings/*` | Search engine rankings (Google, Bing, Local Finder) |
| [Listings API](data-apis/listings.md) | `/data/v1/listings/*` | Find and fetch business profiles from directories |

### Data APIs - Legacy Portal (Batch Method)

These APIs require requests to be submitted within a batch container.

| Document | Endpoint | Description |
|----------|----------|-------------|
| [Reviews API](data-apis/reviews.md) | `/v4/ld/fetch-reviews` | Fetch reviews from 80+ review sites |
| [Offsite SEO API](data-apis/offsite-seo.md) | `/v4/seo/offsite` | Domain-level SEO metrics |
| [Google Business Profile API](data-apis/gbp.md) | `/v4/gpw/*` | GBP reports and rankings |

### Management APIs - Developer Portal (api.brightlocal.com)

These are the newer Management APIs from the Developer Portal.

| Document | Endpoint | Description |
|----------|----------|-------------|
| [Clients API](management-apis/clients.md) | `/manage/v1/clients/*` | Manage client records |
| [Locations API](management-apis/locations.md) | `/manage/v1/locations/*` | Manage business locations |
| [Business Categories API](management-apis/business-categories.md) | `/manage/v1/business-categories/*` | Get business category IDs |

### Management APIs - Legacy Portal (Account Method)

| Document | Endpoint | Description |
|----------|----------|-------------|
| [Citation Tracker API](management-apis/citation-tracker.md) | `/v2/ct/*` | Citation tracking and monitoring |

### Deprecated APIs

| Document | Replacement |
|----------|-------------|
| [Rank Checker API](deprecated/rank-checker.md) | [Rankings API](data-apis/rankings.md) |
| [Citation Builder API](deprecated/citation-builder.md) | Citation Builder API (developer.brightlocal.com) |
| [Deprecated Clients](deprecated/clients.md) | [Clients API](management-apis/clients.md) |
| [Deprecated Locations](deprecated/locations.md) | [Locations API](management-apis/locations.md) |
| [Deprecated Rankings](deprecated/rankings.md) | [Rankings API](data-apis/rankings.md) |

## API Types Overview

### Developer Portal - Async Pattern

```
1. Submit Request → POST /data/v1/rankings/search (get request_id)
2. Wait for Processing → Either poll or use callback_url
3. Retrieve Results → GET /data/v1/rankings/results/{request_id}
```

### Legacy Portal - Batch Method

```
1. Create Batch → POST /v4/batch
2. Add Jobs → POST /v4/ld/fetch-reviews (or other job endpoint)
3. Commit Batch → PUT /v4/batch
4. Poll Results → GET /v4/batch
5. Delete Batch → DELETE /v4/batch
```

### Legacy Portal - Account Method

```
1. Create Report → POST /v2/ct/add (or similar)
2. Run Report → POST /v2/ct/run
3. Get Results → GET /v2/ct/get-results
4. Delete Report → POST /v2/ct/delete
```

## Quick Reference

### Developer Portal APIs (api.brightlocal.com)

| API | Base Path | Type | Rate Limits |
|-----|-----------|------|-------------|
| Rankings | `/data/v1/rankings/*` | Data | POST: 500/min, GET: 600/min |
| Listings | `/data/v1/listings/*` | Data | POST: 500/min, GET: 600/min |
| Clients | `/manage/v1/clients/*` | Management | POST/DELETE: 100/min, GET: 300/min |
| Locations | `/manage/v1/locations/*` | Management | POST/DELETE: 100/min, GET: 300/min |
| Business Categories | `/manage/v1/business-categories/*` | Management | GET: 300/min |

### Legacy Portal APIs (tools.brightlocal.com)

| API | Version | Type | Status |
|-----|---------|------|--------|
| Batches | v4 | Core | Current |
| Reviews | v4 | Batch Method | Current |
| Offsite SEO | v4 | Batch Method | Current |
| Google Business Profile | v4 | Account Method | Current |
| Citation Tracker | v2 | Account Method | Current |

## Supported Countries

AUS, CAN, DEU, GBR, HKG, IRL, MAC, NLD, NZL, PHL, SGP, TWN, USA, ZAF

## Common Error Codes

| Code | Description |
|------|-------------|
| 400 | Bad Request - Invalid parameters |
| 401 | Unauthorized - Invalid API key |
| 404 | Not Found - Resource does not exist |
| 429 | Too Many Requests - Rate limit exceeded |
| INVALID_API_KEY | API key missing or invalid (legacy portal) |
| INVALID_BATCH_ID | Batch ID not found (legacy portal) |
| INVALID_REPORT_ID | Report ID not found (legacy portal) |
| NO_CREDITS | Insufficient credits (legacy portal) |

See [Reference](core/reference.md) for complete error code documentation.

## Notes

- All responses are JSON-encoded
- Trial API keys have limited free credits
- Use callbacks for async notification instead of polling
- The Developer Portal (developer.brightlocal.com) contains newer, recommended APIs
- Legacy portal APIs are still supported but may be deprecated in the future
