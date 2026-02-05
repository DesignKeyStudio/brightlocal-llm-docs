# BrightLocal API Documentation

LLM-optimized markdown documentation for [BrightLocal's Local SEO APIs](https://www.brightlocal.com/local-seo-apis/).

## Table of Contents

- [Overview](#overview)
- [API Portals](#api-portals)
- [Authentication](#authentication)
- [Documentation Index](#documentation-index)
- [Quick Reference](#quick-reference)
- [API Patterns](#api-patterns)
- [Supported Countries](#supported-countries)
- [Error Codes](#error-codes)
- [Sources](#sources)

## Overview

BrightLocal provides APIs for local SEO data including:

- **Reviews** - Fetch reviews from 80+ sites (Google, Yelp, Facebook, etc.)
- **Rankings** - Track search rankings (Google, Bing, Local Pack)
- **Listings** - Find and fetch business profiles from directories
- **Citations** - Track and manage business citations
- **Google Business Profile** - GBP data and competitive analysis
- **Domain Metrics** - Backlinks, domain authority, indexed pages

## API Portals

BrightLocal maintains two API systems:

| Portal | Documentation | API Base URL | Auth Method |
|--------|---------------|--------------|-------------|
| **Developer Portal** | [developer.brightlocal.com](https://developer.brightlocal.com/) | `https://api.brightlocal.com` | `x-api-key` header |
| **Legacy Portal** | [apidocs.brightlocal.com](https://apidocs.brightlocal.com/) | `https://tools.brightlocal.com/seo-tools/api` | `api-key` parameter |

## Authentication

### Developer Portal

```bash
curl -X POST "https://api.brightlocal.com/data/v1/rankings/search" \
  -H "x-api-key: YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"search_engine": "google", "search_term": "pizza", "country": "USA"}'
```

### Legacy Portal

```bash
curl -X POST "https://tools.brightlocal.com/seo-tools/api/v4/batch" \
  -d "api-key=YOUR_API_KEY"
```

## Documentation Index

### Core

| Document | Description |
|----------|-------------|
| [Getting Started](core/getting-started.md) | Authentication, base URLs, API types, quickstart |
| [Batches](core/batches.md) | Batch processing for async requests (Legacy Portal) |
| [Reference](core/reference.md) | Countries, directories, error codes |

### Data APIs

| Document | Portal | Endpoint | Description |
|----------|--------|----------|-------------|
| [Rankings](data-apis/rankings.md) | Developer | `/data/v1/rankings/*` | Search rankings (Google, Bing, Local) |
| [Listings](data-apis/listings.md) | Developer | `/data/v1/listings/*` | Business profiles from directories |
| [Reviews](data-apis/reviews.md) | Legacy | `/v4/ld/fetch-reviews` | Reviews from 80+ sites |
| [Offsite SEO](data-apis/offsite-seo.md) | Legacy | `/v4/seo/offsite` | Domain authority, backlinks |
| [Google Business Profile](data-apis/gbp.md) | Legacy | `/v4/gpw/*` | GBP reports and rankings |

### Management APIs

| Document | Portal | Endpoint | Description |
|----------|--------|----------|-------------|
| [Clients](management-apis/clients.md) | Developer | `/manage/v1/clients/*` | Client records |
| [Locations](management-apis/locations.md) | Developer | `/manage/v1/locations/*` | Business locations |
| [Business Categories](management-apis/business-categories.md) | Developer | `/manage/v1/business-categories/*` | Category IDs |
| [Citation Tracker](management-apis/citation-tracker.md) | Legacy | `/v2/ct/*` | Citation monitoring |

### Deprecated APIs

| Document | Replacement |
|----------|-------------|
| [Rank Checker](deprecated/rank-checker.md) | [Rankings API](data-apis/rankings.md) |
| [Citation Builder](deprecated/citation-builder.md) | New Citation Builder API |
| [Reputation Manager](deprecated/reputation-manager.md) | New Reputation Manager API |
| [Local Directories](deprecated/local-directories.md) | [Listings API](data-apis/listings.md) |
| [Old Rankings](deprecated/rankings.md) | [Rankings API](data-apis/rankings.md) |
| [Old Clients](deprecated/clients.md) | [Clients API](management-apis/clients.md) |
| [Old Locations](deprecated/locations.md) | [Locations API](management-apis/locations.md) |

See [Deprecated APIs Index](deprecated/README.md) for migration guidance.

## Quick Reference

### Developer Portal Rate Limits

| API | POST | GET |
|-----|------|-----|
| Rankings | 500/min | 600/min |
| Listings | 500/min | 600/min |
| Clients | 100/min | 300/min |
| Locations | 100/min | 300/min |
| Business Categories | - | 300/min |

### Legacy Portal APIs

| API | Version | Type | Method |
|-----|---------|------|--------|
| Batches | v4 | Core | Batch container |
| Reviews | v4 | Data | Batch method |
| Offsite SEO | v4 | Data | Batch method |
| GBP | v4 | Management | Account method |
| Citation Tracker | v2 | Management | Account method |

## API Patterns

### Developer Portal (Async)

```
1. POST request → receive request_id
2. Poll GET endpoint OR wait for callback_url
3. GET results using request_id
```

### Legacy Portal (Batch Method)

```
1. POST /v4/batch → create batch, get batch-id
2. POST /v4/ld/fetch-reviews → add job to batch
3. PUT /v4/batch → commit batch
4. GET /v4/batch → poll for results
5. DELETE /v4/batch → cleanup
```

### Legacy Portal (Account Method)

```
1. POST /v2/ct/add → create report
2. POST /v2/ct/run → execute report
3. GET /v2/ct/get-results → fetch results
```

## Supported Countries

| Code | Country |
|------|---------|
| AUS | Australia |
| CAN | Canada |
| DEU | Germany |
| GBR | United Kingdom |
| HKG | Hong Kong |
| IRL | Ireland |
| MAC | Macau |
| NLD | Netherlands |
| NZL | New Zealand |
| PHL | Philippines |
| SGP | Singapore |
| TWN | Taiwan |
| USA | United States |
| ZAF | South Africa |

## Error Codes

### HTTP Status Codes

| Code | Description |
|------|-------------|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request - Invalid parameters |
| 401 | Unauthorized - Invalid API key |
| 404 | Not Found |
| 405 | Method Not Allowed |
| 429 | Rate Limit Exceeded |
| 500 | Server Error |

### Legacy Portal Error Codes

| Code | Description |
|------|-------------|
| INVALID_API_KEY | API key missing or invalid |
| INVALID_BATCH_ID | Batch ID not found |
| INVALID_REPORT_ID | Report ID not found |
| NO_CREDITS | Insufficient credits |
| REPORT_RUNNING | Report already executing |

See [Reference](core/reference.md) for complete error documentation.

## Sources

This documentation was extracted from:

- [BrightLocal Developer Portal](https://developer.brightlocal.com/) - New APIs
- [BrightLocal API Reference](https://apidocs.brightlocal.com/) - Legacy APIs
- [BrightLocal Local SEO APIs](https://www.brightlocal.com/local-seo-apis/) - Overview

## Notes

- All responses are JSON
- Trial API keys have limited credits
- Developer Portal APIs are recommended for new integrations
- Legacy Portal APIs remain supported for Reviews, Offsite SEO, GBP, and Citation Tracker
