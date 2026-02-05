# Rankings API

> **Status**: Current
> **Portal**: developer.brightlocal.com
> **Base URL**: `https://api.brightlocal.com`
> **Version**: v1.0

## Overview

The BrightLocal Rankings API helps you understand where and how a business ranks in search engines. It supports:
- Google
- Google (as Mobile)
- Google Local Finder
- Bing
- Bing Local

You can return full SERP results, raw HTML, and screenshots of the SERP pages.

## Authentication

Include your API key in the `x-api-key` header:

```bash
curl --header 'x-api-key: YOUR_API_KEY'
```

## Rate Limits

| Request Type | Limit |
|--------------|-------|
| POST | 500 requests/minute |
| GET | 600 requests/minute |

## Endpoints

### Find Locations

Find canonical location names for use with search ranking requests.

| | |
|---|---|
| **Method** | POST |
| **Endpoint** | `/data/v1/locations/search` |

#### Request Body

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| query | string | Yes | Location search query |
| country | string | Yes | ISO 3166-1 country code (e.g., USA) |

#### Example Request

```bash
curl --request POST \
  --url https://api.brightlocal.com/data/v1/locations/search \
  --header 'Accept: application/json' \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: YOUR_API_KEY' \
  --data '{
    "query": "New York",
    "country": "USA"
  }'
```

#### Example Response

```json
{
  "locations": [
    {
      "name": "New York, New York, United States",
      "canonical_name": "New York,New York,United States"
    }
  ]
}
```

---

### Create Search Rankings Request

Submit a search ranking request. This endpoint is asynchronous and returns a `request_id`.

| | |
|---|---|
| **Method** | POST |
| **Endpoint** | `/data/v1/rankings/search` |

#### Request Body

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| search_engine | string | Yes | Engine: `google`, `google-mobile`, `google-local-finder`, `bing`, `bing-local` |
| search_term | string | Yes | Search query (<= 400 characters) |
| country | string | Yes | ISO 3166-1 country code (e.g., USA) |
| geo_location | object | Yes | Location for the search |
| geo_location.name | string | Yes | Canonical or freeform location name |
| geo_location.coordinates | object | No | Latitude/longitude coordinates |
| lang | string | No | ISO 639-1 language code (default: "en") |
| num_results | integer | No | Max results (1-100, default: 50) |
| match_criteria | object | No | Criteria to match against search results |
| match_criteria.website_urls | array | No | URLs to match (<= 10 items) |
| match_criteria.uid | string | No | Google ludocid or Bing Local ID |
| match_criteria.nap | object | No | Name, address, phone matching |
| callback_url | string | No | URL for async result notification |

#### Example Request

```bash
curl --request POST \
  --url https://api.brightlocal.com/data/v1/rankings/search \
  --header 'Accept: application/json' \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: YOUR_API_KEY' \
  --data '{
    "search_engine": "google",
    "search_term": "pizza near me",
    "country": "USA",
    "lang": "en",
    "geo_location": {
      "name": "Woodstock, Georgia, United States"
    },
    "num_results": 10,
    "match_criteria": {
      "website_urls": [
        "https://www.yelp.com",
        "http://apple.com"
      ],
      "uid": "14242700967722301077",
      "nap": {
        "names": ["Domino's"],
        "telephone": "+1 770-591-3331",
        "postcode": "30189",
        "use_for": {
          "organic": true,
          "local": true
        }
      }
    },
    "callback_url": "http://example.com/callback"
  }'
```

#### Example Response

```json
{
  "request_id": "569f8f95-8195-443a-af4e-3eb3e2efc77d"
}
```

#### Response Codes

| Code | Description |
|------|-------------|
| 201 | Created - Request submitted successfully |
| 400 | Bad Request - Invalid parameters |
| 401 | Unauthorized - Invalid API key |
| 429 | Too Many Requests - Rate limit exceeded |
| 500 | Internal Server Error |

---

### Get Rankings Results

Retrieve results for a search rankings request.

| | |
|---|---|
| **Method** | GET |
| **Endpoint** | `/data/v1/rankings/results/{request_id}` |

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| request_id | string | Yes | The request ID from the create request |

#### Example Request

```bash
curl --request GET \
  --url https://api.brightlocal.com/data/v1/rankings/results/569f8f95-8195-443a-af4e-3eb3e2efc77d \
  --header 'Accept: application/json' \
  --header 'x-api-key: YOUR_API_KEY'
```

---

### Get HTML

Retrieve the raw HTML from the search results page.

| | |
|---|---|
| **Method** | GET |
| **Endpoint** | `/data/v1/rankings/html/{request_id}/{page}` |

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| request_id | string | Yes | The request ID |
| page | integer | Yes | Page number |

---

### Get Screenshot

Retrieve a screenshot of the search results page.

| | |
|---|---|
| **Method** | GET |
| **Endpoint** | `/data/v1/rankings/screenshots/{request_id}/{page}` |

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| request_id | string | Yes | The request ID |
| page | integer | Yes | Page number |

---

### Create MSV Request

Create a Monthly Search Volume (MSV) request for keywords.

| | |
|---|---|
| **Method** | POST |
| **Endpoint** | `/data/v1/msv/search` |

#### Request Body

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| keywords | array | Yes | Keywords to get search volume for |
| country | string | Yes | ISO 3166-1 country code |
| callback_url | string | No | URL for async notification |

#### Example Response

```json
{
  "request_id": "abc123-def456"
}
```

---

### Get MSV Results

Retrieve Monthly Search Volume results.

| | |
|---|---|
| **Method** | GET |
| **Endpoint** | `/data/v1/msv/results/{request_id}` |

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| request_id | string | Yes | The request ID from the MSV request |

## Asynchronous Workflow

The Rankings API uses an asynchronous pattern:

1. **Submit Request** - POST to create a search request, receive `request_id`
2. **Wait for Processing** - Either poll GET endpoint or wait for callback
3. **Retrieve Results** - GET results using the `request_id`

### Using Callbacks

Instead of polling, provide a `callback_url` in your request. BrightLocal will POST results to your URL when processing completes.

```json
{
  "search_engine": "google",
  "search_term": "plumber",
  "country": "USA",
  "geo_location": {"name": "New York, NY"},
  "callback_url": "https://your-server.com/webhook"
}
```

## Search Engines Reference

| Engine | Description |
|--------|-------------|
| google | Google web search |
| google-mobile | Google mobile search |
| google-local-finder | Google Local Finder/Maps |
| bing | Bing web search |
| bing-local | Bing local/maps search |

## Match Criteria

For accurate ranking detection, provide match criteria:

- **website_urls**: Match against website URLs in results
- **uid**: Google's ludocid (recommended for Local Pack/Finder matching)
- **nap**: Name, Address, Phone for business matching

```json
{
  "match_criteria": {
    "website_urls": ["https://yourbusiness.com"],
    "uid": "14242700967722301077",
    "nap": {
      "names": ["Your Business Name"],
      "telephone": "+1-555-555-5555",
      "postcode": "12345"
    }
  }
}
```
