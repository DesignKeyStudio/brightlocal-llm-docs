# Listings API

> **Status**: Current
> **Portal**: developer.brightlocal.com
> **Base URL**: `https://api.brightlocal.com`
> **Version**: v1.0

## Overview

The BrightLocal Listings API allows you to find and fetch business profiles from various directories. It supports searching for business listings across multiple platforms and retrieving detailed profile information.

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

### Find Profile

Search for a business listing on a specified directory. This endpoint is asynchronous and returns a `request_id`.

| | |
|---|---|
| **Method** | POST |
| **Endpoint** | `/data/v1/listings/find` |

#### Request Body

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| business_names | array | Yes | List of business names to search for |
| country | string | Yes | ISO 3166-1 country code (e.g., GBR, USA) |
| directory | string | Yes | Directory to search (e.g., google, yelp, facebook) |
| region | string | No | State/province/region name |
| city | string | No | City name |
| postcode | string | No | Postal/ZIP code |
| telephone | string | No | Business phone number |
| street_address | string | No | Street address |
| callback_url | string | No | URL for async result notification |

#### Example Request

```bash
curl --request POST \
  --url https://api.brightlocal.com/data/v1/listings/find \
  --header 'Accept: application/json' \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: YOUR_API_KEY' \
  --data '{
    "business_names": [
      "BrightLocal",
      "The Oracle"
    ],
    "country": "GBR",
    "region": "Brighton and Hove",
    "city": "Brighton",
    "postcode": "BN1 1EB",
    "directory": "google",
    "telephone": "+441273605027",
    "street_address": "First Floor, Huntingdon House, 20 North St"
  }'
```

#### Example Response

```json
{
  "request_id": "0191ec4c-139a-7da8-8d9f-05920e9a869e"
}
```

#### Response Codes

| Code | Description |
|------|-------------|
| 201 | Created - Request submitted successfully |
| 400 | Bad Request - Invalid parameters |
| 401 | Unauthorized - Invalid API key |

**Note**: Complete as many fields as possible to increase the chances of finding the correct profile.

---

### Fetch Profile

Fetch a business profile by its URL. This endpoint is asynchronous.

| | |
|---|---|
| **Method** | POST |
| **Endpoint** | `/data/v1/listings/fetch` |

#### Request Body

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| url | string | Yes | Direct URL to the business profile |
| directory | string | Yes | Directory the URL belongs to |
| callback_url | string | No | URL for async result notification |

#### Example Request

```bash
curl --request POST \
  --url https://api.brightlocal.com/data/v1/listings/fetch \
  --header 'Accept: application/json' \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: YOUR_API_KEY' \
  --data '{
    "url": "https://www.google.com/maps/place/BrightLocal/@50.8225,-0.1372",
    "directory": "google",
    "callback_url": "https://your-server.com/webhook"
  }'
```

#### Example Response

```json
{
  "request_id": "0191ec4c-139a-7da8-8d9f-05920e9a869f"
}
```

---

### Get Results

Retrieve results for a listings find or fetch request.

| | |
|---|---|
| **Method** | GET |
| **Endpoint** | `/data/v1/listings/results/{request_id}` |

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| request_id | string | Yes | The request ID from Find or Fetch request |

#### Example Request

```bash
curl --request GET \
  --url https://api.brightlocal.com/data/v1/listings/results/0191ec4c-139a-7da8-8d9f-05920e9a869e \
  --header 'Accept: application/json' \
  --header 'x-api-key: YOUR_API_KEY'
```

---

### Get All Directories

Retrieve a list of all supported directories.

| | |
|---|---|
| **Method** | GET |
| **Endpoint** | `/data/v1/listings/directories` |

#### Example Request

```bash
curl --request GET \
  --url https://api.brightlocal.com/data/v1/listings/directories \
  --header 'Accept: application/json' \
  --header 'x-api-key: YOUR_API_KEY'
```

---

### Get All Directories by Country

Retrieve directories supported for a specific country.

| | |
|---|---|
| **Method** | GET |
| **Endpoint** | `/data/v1/listings/directories/{country}` |

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| country | string | Yes | ISO 3166-1 country code (e.g., USA, GBR) |

#### Example Request

```bash
curl --request GET \
  --url https://api.brightlocal.com/data/v1/listings/directories/USA \
  --header 'Accept: application/json' \
  --header 'x-api-key: YOUR_API_KEY'
```

## Asynchronous Workflow

The Listings API uses an asynchronous pattern:

1. **Submit Request** - POST to find or fetch a profile, receive `request_id`
2. **Wait for Processing** - Either poll GET endpoint or wait for callback
3. **Retrieve Results** - GET results using the `request_id`

### Using Callbacks

Instead of polling, provide a `callback_url` in your request:

```json
{
  "business_names": ["My Business"],
  "country": "USA",
  "directory": "google",
  "callback_url": "https://your-server.com/webhook"
}
```

BrightLocal will POST results to your URL when processing completes.

## Supported Directories

Use the Get All Directories endpoint to retrieve the current list. Common directories include:

| Directory | Description |
|-----------|-------------|
| google | Google Business Profile |
| bing | Bing Places |
| yelp | Yelp |
| facebook | Facebook Business |
| foursquare | Foursquare |
| yellowpages | Yellow Pages |
| bbb | Better Business Bureau |

## Best Practices

1. **Provide Complete Information**: Include as many fields as possible (name, address, phone, city, postcode) to improve matching accuracy

2. **Use Correct Country Codes**: Use ISO 3166-1 alpha-3 country codes (USA, GBR, CAN, etc.)

3. **Handle Multiple Names**: If a business uses variations of its name, include them all in the `business_names` array

4. **Implement Callbacks**: For production use, implement callbacks instead of polling to reduce API calls

## Supported Countries

The Listings API supports:
- AUS (Australia)
- CAN (Canada)
- DEU (Germany)
- GBR (United Kingdom)
- HKG (Hong Kong)
- IRL (Ireland)
- MAC (Macau)
- NLD (Netherlands)
- NZL (New Zealand)
- PHL (Philippines)
- SGP (Singapore)
- TWN (Taiwan)
- USA (United States)
- ZAF (South Africa)
