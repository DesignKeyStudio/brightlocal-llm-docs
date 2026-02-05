# Citation Tracker API

> **Status**: Current

## Overview

The Citation Tracker API allows you to manage citation tracking reports for local businesses. Track business citations across directories, compare performance against competitors, and monitor NAP (Name, Address, Phone) consistency.

This is an **Account Method** API - reports are stored in your BrightLocal account.

## Authentication

- API key required via `api-key` parameter
- Base URL: `https://tools.brightlocal.com/seo-tools/api`

## Endpoints

### Add Report

**`POST /v2/ct/add`**

Creates a new citation tracker report.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| location-id | integer | Yes | Location identifier |
| business-type | string | Yes | Category (e.g., "Restaurant") |
| primary-location | string | Yes | City/town or ZIP code for competitor identification |
| monthly-run | integer | No | Day of month to run (1-31, 0 for none) |
| weekly-run | integer | No | Day of week to run (1-7, 0 for none) |
| notify | string | No | Send alerts: "yes" or "no" |
| email-addresses | array | No | JSON array of emails (max 5) |

#### Request Example

```bash
curl -X POST "https://tools.brightlocal.com/seo-tools/api/v2/ct/add" \
  -d "api-key=YOUR_API_KEY" \
  -d "location-id=1000" \
  -d "business-type=Restaurant" \
  -d "primary-location=Chicago, IL" \
  -d "monthly-run=15" \
  -d "notify=yes" \
  -d 'email-addresses=["user@example.com"]'
```

#### Response Example

```json
{
  "response": {
    "status": "added",
    "report-id": 682
  }
}
```

---

### Update Report

**`POST /v2/ct/update`**

Updates an existing citation tracker report.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| location-id | integer | Yes | Location identifier |
| report-id | integer | Yes | Report to update |
| business-type | string | Yes | Business category |
| primary-location | string | No | City/town or ZIP code |
| monthly-run | integer | No | Day of month (1-31, 0 for none) |
| weekly-run | integer | No | Day of week (1-7, 0 for none) |
| notify | string | No | "yes" or "no" |
| email-addresses | array | No | JSON array of emails |

#### Request Example

```bash
curl -X POST "https://tools.brightlocal.com/seo-tools/api/v2/ct/update" \
  -d "api-key=YOUR_API_KEY" \
  -d "location-id=1000" \
  -d "report-id=682" \
  -d "business-type=Italian Restaurant" \
  -d "monthly-run=1"
```

#### Response Example

```json
{
  "response": {
    "status": "edited"
  }
}
```

---

### Get Report

**`GET /v2/ct/get`**

Retrieves report configuration and run history.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| report-id | integer | Yes | Report ID |

#### Request Example

```bash
curl -X GET "https://tools.brightlocal.com/seo-tools/api/v2/ct/get?api-key=YOUR_API_KEY&report-id=682"
```

#### Response Example

```json
{
  "response": {
    "report-id": 682,
    "location-id": 1000,
    "business-type": "Restaurant",
    "primary-location": "Chicago, IL",
    "monthly-run": 15,
    "weekly-run": 0,
    "notify": "yes",
    "email-addresses": ["user@example.com"],
    "last-run": "2024-01-15",
    "status": "completed"
  }
}
```

---

### Run Report

**`POST /v2/ct/run`**

Manually triggers a report execution.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| report-id | integer | Yes | Report ID to execute |

#### Request Example

```bash
curl -X POST "https://tools.brightlocal.com/seo-tools/api/v2/ct/run" \
  -d "api-key=YOUR_API_KEY" \
  -d "report-id=682"
```

#### Response Example

```json
{
  "response": {
    "status": "running"
  }
}
```

---

### Delete Report

**`POST /v2/ct/delete`**

Permanently removes a citation tracker report.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| report-id | integer | Yes | Report ID to delete |

#### Request Example

```bash
curl -X POST "https://tools.brightlocal.com/seo-tools/api/v2/ct/delete" \
  -d "api-key=YOUR_API_KEY" \
  -d "report-id=682"
```

#### Response Example

```json
{
  "response": {
    "status": "deleted"
  }
}
```

> **Warning**: This action cannot be undone.

---

### Get All Reports

**`GET /v2/ct/get-all`**

Lists all citation tracker reports.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| location-id | integer | No | Filter by location |

#### Request Example

```bash
curl -X GET "https://tools.brightlocal.com/seo-tools/api/v2/ct/get-all?api-key=YOUR_API_KEY"
```

#### Response Example

```json
{
  "response": {
    "results": [
      {
        "report-id": 682,
        "location-id": 1000,
        "business-type": "Restaurant",
        "status": "completed",
        "active-citations": 45,
        "pending-citations": 3
      }
    ]
  }
}
```

---

### Get Report Results

**`GET /v2/ct/get-results`**

Retrieves detailed citation tracking results.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| report-id | integer | Yes | Report ID |

#### Request Example

```bash
curl -X GET "https://tools.brightlocal.com/seo-tools/api/v2/ct/get-results?api-key=YOUR_API_KEY&report-id=682"
```

#### Response Example

```json
{
  "response": {
    "summary": {
      "active-citations": 45,
      "pending-citations": 3,
      "possible-citations": 12,
      "total-citations": 60
    },
    "citations": [
      {
        "directory": "yelp.com",
        "status": "active",
        "domain-authority": 94,
        "nap-match": true,
        "url": "https://yelp.com/biz/example"
      },
      {
        "directory": "yellowpages.com",
        "status": "active",
        "domain-authority": 87,
        "nap-match": false,
        "url": "https://yellowpages.com/..."
      }
    ],
    "competitors": [
      {
        "name": "Competitor A",
        "active-citations": 52,
        "domain-authority-avg": 78
      }
    ],
    "top-directories": [
      {
        "directory": "google.com",
        "authority": 100,
        "status": "active"
      }
    ]
  }
}
```

## Results Fields

| Field | Description |
|-------|-------------|
| active-citations | Confirmed live citations |
| pending-citations | Citations being verified |
| possible-citations | Potential citation opportunities |
| domain-authority | Authority score of directory (0-100) |
| nap-match | Whether NAP data matches exactly |
| competitors | Competitor comparison data |

## Error Codes

| Error Code | Description |
|------------|-------------|
| INVALID_API_KEY | API key is missing or invalid |
| INVALID_REPORT_ID | Report ID not found |
| INVALID_LOCATION_ID | Location ID not found |
| REPORT_RUNNING | Report already executing |

## Notes

- Tracks citations across major directories
- Compares citation performance vs competitors
- Monitors NAP (Name, Address, Phone) consistency
- Includes domain authority metrics
- Supports scheduled weekly or monthly execution
- Email notifications for changes available
