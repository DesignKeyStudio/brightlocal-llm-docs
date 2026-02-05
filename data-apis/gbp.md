# Google Business Profile (GBP) API

> **Status**: Current

## Overview

The Google Business Profile API (formerly Google My Business API) allows you to manage GBP reports, track local rankings, and retrieve competitive analysis data for Google Business Profiles.

This is an **Account Method** API - reports are stored in your BrightLocal account.

## Authentication

- API key required via `api-key` parameter
- Base URL: `https://tools.brightlocal.com/seo-tools/api`

## Endpoints

### Add Report

**`POST /v4/gpw/add`**

Creates a new Google Business Profile report.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| location_id | integer | Yes | Associates report with account location |
| schedule | string | Yes | "Adhoc" or "Monthly" |
| day_of_month | integer | Yes | Day to run (1-28, or -1 for last day) |
| report_type | string | Yes | "with" (has Google profile) or "without" |
| google_location | string | Yes | Valid Google search location (e.g., "Chicago, IL") |
| search_terms | array | Yes | JSON array of 1-5 search terms |
| notify | string | No | Email notifications: "Yes" or "No" (default: "No") |
| email_addresses | array | No | JSON array of emails (max 5) |
| run | string | No | Execute after creation: "Yes" or "No" (default: "Yes") |

#### Request Example

```bash
curl -X POST "https://tools.brightlocal.com/seo-tools/api/v4/gpw/add" \
  -d "api-key=YOUR_API_KEY" \
  -d "location_id=1000" \
  -d "schedule=Monthly" \
  -d "day_of_month=15" \
  -d "report_type=with" \
  -d "google_location=Chicago, IL" \
  -d 'search_terms=["pizza delivery","best pizza near me"]' \
  -d "notify=Yes" \
  -d 'email_addresses=["user@example.com"]'
```

#### Response Example (201 Created)

```json
{
  "success": true,
  "report-id": "1"
}
```

---

### Update Report

**`PUT /v4/gpw/{reportId}`**

Updates an existing report configuration.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| report-id | integer | Yes | Report ID (in URL path) |
| location_id | integer | Yes | Location ID |
| schedule | string | No | "Adhoc" or "Monthly" |
| day_of_month | integer | No | Day to run (1-28 or -1) |
| search_terms | array | No | JSON array of search terms |
| notify | string | No | "Yes" or "No" |
| email_addresses | array | No | JSON array of emails |

#### Request Example

```bash
curl -X PUT "https://tools.brightlocal.com/seo-tools/api/v4/gpw/1" \
  -d "api-key=YOUR_API_KEY" \
  -d "location_id=1000" \
  -d "schedule=Adhoc"
```

#### Response Example (200 OK)

```json
{
  "success": true
}
```

---

### Get Report

**`GET /v4/gpw/{reportId}`**

Retrieves report configuration details.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |

#### Request Example

```bash
curl -X GET "https://tools.brightlocal.com/seo-tools/api/v4/gpw/1?api-key=YOUR_API_KEY"
```

#### Response Example (200 OK)

```json
{
  "success": true,
  "report": {
    "report_id": "1",
    "report_name": "Chicago Pizza Rankings",
    "location_id": "1000",
    "schedule": "Monthly",
    "report_type": "with",
    "search_terms": ["pizza delivery", "best pizza near me"],
    "google_location": "Chicago, IL",
    "notify": "Yes",
    "is_running": "No"
  }
}
```

---

### Delete Report

**`DELETE /v4/gpw/{reportId}`**

Permanently removes a report.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |

#### Request Example

```bash
curl -X DELETE "https://tools.brightlocal.com/seo-tools/api/v4/gpw/1" \
  -d "api-key=YOUR_API_KEY"
```

#### Response Example (200 OK)

```json
{
  "success": true
}
```

> **Warning**: This action cannot be undone.

---

### Get All Reports

**`GET /v4/gpw`**

Lists all GBP reports in your account.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| location-id | integer | No | Filter by location ID |

#### Request Example

```bash
curl -X GET "https://tools.brightlocal.com/seo-tools/api/v4/gpw?api-key=YOUR_API_KEY"
```

#### Response Example (200 OK)

```json
{
  "response": {
    "results": [
      {
        "report_id": "49",
        "report_name": "Test Report 1",
        "schedule": "Weekly",
        "is_running": "Yes"
      },
      {
        "report_id": "50",
        "report_name": "Test Report 2",
        "schedule": "Monthly",
        "is_running": "No"
      }
    ]
  }
}
```

---

### Run Report

**`POST /v4/gpw/run`**

Manually triggers a report execution.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| report-id | integer | Yes | Report ID to execute |

#### Request Example

```bash
curl -X POST "https://tools.brightlocal.com/seo-tools/api/v4/gpw/run" \
  -d "api-key=YOUR_API_KEY" \
  -d "report-id=1"
```

#### Response Example (200 OK)

```json
{
  "success": true
}
```

#### Error Responses

| Error Code | Description |
|------------|-------------|
| INVALID_REPORT_ID | Report ID missing or invalid |
| NO_CREDITS | Insufficient account credits |
| REPORT_RUNNING | Report already executing |

---

### Get Report Results

**`GET /v4/gpw/{reportId}/results`**

Retrieves comprehensive report results including rankings and competitive data.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |

#### Request Example

```bash
curl -X GET "https://tools.brightlocal.com/seo-tools/api/v4/gpw/1/results?api-key=YOUR_API_KEY"
```

#### Response Structure

```json
{
  "success": true,
  "results": {
    "summary": {
      "business_name": "Example Business",
      "rating": 4.5,
      "review_count": 150,
      "citations_count": 45
    },
    "keywords": {
      "pizza delivery": {
        "rank": 3,
        "previous_rank": 5,
        "change": "+2"
      }
    },
    "top_10": [
      {
        "rank": 1,
        "business_name": "Competitor A",
        "rating": 4.8,
        "review_count": 200
      }
    ],
    "citations_matrix": {
      "yelp.com": { "status": "found", "authority": 94 },
      "yellowpages.com": { "status": "found", "authority": 87 }
    },
    "nap_comparison": {
      "name_match": true,
      "address_match": true,
      "phone_match": false
    },
    "urls": {
      "interactive": "https://...",
      "pdf": "https://...",
      "csv": "https://..."
    }
  }
}
```

## Results Fields

| Field | Description |
|-------|-------------|
| summary | Business info, ratings, citation count |
| keywords | Rankings data organized by search term |
| top_10 | Competitor rankings with detailed metrics |
| citations_matrix | Citation data by domain with authority scores |
| nap_comparison | Name/Address/Phone consistency check |
| urls | Links to interactive, PDF, and CSV reports |

## Error Codes

| Error Code | Description |
|------------|-------------|
| INVALID_API_KEY | API key is missing or invalid |
| INVALID_REPORT_ID | Report ID not found |
| INVALID_LOCATION_ID | Location ID not found |
| NO_CREDITS | Insufficient credits |
| REPORT_RUNNING | Report already in progress |

## Notes

- Reports can be scheduled monthly or run on-demand (Adhoc)
- Maximum 5 search terms per report
- Email notifications support up to 5 addresses
- Results include domain authority scores and backlink data
- This API is documented in the older portal but remains current
