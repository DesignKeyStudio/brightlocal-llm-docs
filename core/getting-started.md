# Getting Started with BrightLocal API

> **Status**: Current

## Introduction

The BrightLocal API provides programmatic access to Local SEO Tools through a REST-style interface. All responses are returned as JSON.

**Important Note**: BrightLocal maintains two API documentation portals:
- **Legacy Portal** (`apidocs.brightlocal.com`) - Contains older APIs, many deprecated
- **Developer Portal** (`developer.brightlocal.com`) - Newer APIs, recommended for new integrations

This documentation covers both portals to provide comprehensive API coverage.

## Base URL

```
https://tools.brightlocal.com/seo-tools/api
```

All API endpoints are relative to this base URL.

## Authentication

All API methods require authentication via an API key.

### API Key Parameter

Include your API key as a parameter in every request:

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your BrightLocal API key |

### Request Example

```bash
# GET request
curl -X GET "https://tools.brightlocal.com/seo-tools/api/v4/batch?api-key=YOUR_API_KEY&batch-id=17"

# POST request
curl -X POST "https://tools.brightlocal.com/seo-tools/api/v4/batch" \
  -d "api-key=YOUR_API_KEY" \
  -d "stop-on-job-error=0"
```

### Authentication Errors

| Error Message | Description |
|---------------|-------------|
| "Invalid API key specified" | API key is invalid or lacks access to the requested method |
| "No trial requests left" | Trial API key has exhausted its free credit allocation |

### API Key Types

1. **Trial API Key** - Limited free credits for testing
2. **Production API Key** - Full access for production use

Contact BrightLocal support to enable specific API methods for your key if you receive "Invalid API key specified" errors.

## API Versioning

The BrightLocal API uses versioned endpoints:

| Version | URL Pattern | Notes |
|---------|-------------|-------|
| v4 | `/v4/endpoint` | Current version for most APIs |
| v2 | `/v2/endpoint` | Used by Citation Tracker API |

Always use the latest available version for each API endpoint.

## API Types

BrightLocal APIs are categorized into two types:

### 1. Batch Method APIs

These APIs process requests asynchronously within a batch container:

- **Reviews API** - Fetch review data
- **Offsite SEO API** - Domain-level SEO metrics

**Workflow:**
1. Create a batch (get `batch-id`)
2. Add jobs to the batch
3. Commit the batch to start processing
4. Poll for results

### 2. Account Method APIs

These APIs manage resources stored in your BrightLocal account:

- **Citation Tracker API** - Manage citation tracking reports
- **Google Business Profile API** - Manage GBP reports

**Workflow:**
1. Create/update reports using `location-id`
2. Run reports on-demand or schedule them
3. Retrieve results

## Response Format

All API responses are JSON-encoded.

### Success Response

```json
{
  "success": true,
  "response": {
    "data": "..."
  }
}
```

### Error Response

```json
{
  "success": false,
  "errors": {
    "ERROR_CODE": "Error description"
  }
}
```

Multiple validation errors may be returned together.

## Rate Limiting

BrightLocal does not publish explicit rate limits. However, best practices include:

- Submit batch jobs in groups of several hundred maximum
- Implement exponential backoff for retries
- Poll batch results every 5 seconds after commitment
- Contact support if you need high-volume access

## Callback/Webhook Support

Batch APIs support asynchronous notifications via callbacks:

| Parameter | Type | Description |
|-----------|------|-------------|
| callback | string | URL to receive completion notifications |
| callback_request_format | string | Format: 'form' or 'json' (default: 'form') |

When a batch completes or fails, BrightLocal posts the results to your callback URL.

### Callback Example

```bash
curl -X POST "https://tools.brightlocal.com/seo-tools/api/v4/batch" \
  -d "api-key=YOUR_API_KEY" \
  -d "callback=https://your-server.com/webhook" \
  -d "callback_request_format=json"
```

## Quick Start Guide

### Step 1: Get Your API Key

1. Log into your BrightLocal account
2. Navigate to API settings
3. Generate or copy your API key

### Step 2: Test Authentication

```bash
curl -X POST "https://tools.brightlocal.com/seo-tools/api/v4/batch" \
  -d "api-key=YOUR_API_KEY"
```

Expected response:
```json
{
  "success": true,
  "batch-id": "123"
}
```

### Step 3: Clean Up Test Batch

```bash
curl -X DELETE "https://tools.brightlocal.com/seo-tools/api/v4/batch" \
  -d "api-key=YOUR_API_KEY" \
  -d "batch-id=123"
```

## HTTP Methods

| Method | Usage |
|--------|-------|
| GET | Retrieve data (reports, results) |
| POST | Create resources, add jobs |
| PUT | Update resources, commit batches |
| DELETE | Remove resources |

## Content Type

All POST/PUT requests should use `application/x-www-form-urlencoded` content type.

```bash
curl -X POST "https://tools.brightlocal.com/seo-tools/api/v4/batch" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "api-key=YOUR_API_KEY"
```

## Next Steps

- **Batch Processing**: See [Batches API](batches.md) for batch workflow details
- **Reviews Data**: See [Reviews API](../data-apis/reviews.md) for fetching reviews
- **Citation Tracking**: See [Citation Tracker API](../management-apis/citation-tracker.md) for citation management
- **Reference Data**: See [Reference](reference.md) for supported countries and directories
