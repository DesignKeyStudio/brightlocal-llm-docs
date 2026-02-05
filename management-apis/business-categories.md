# Business Categories API

> **Status**: Current
> **Portal**: developer.brightlocal.com
> **Base URL**: `https://api.brightlocal.com`
> **Version**: v1.0

## Overview

The Business Categories API provides access to BrightLocal's supported business categories. Use this API to retrieve valid category IDs for use when creating or updating locations.

## Authentication

Include your API key in the `x-api-key` header:

```bash
curl --header 'x-api-key: YOUR_API_KEY'
```

## Rate Limits

| Request Type | Limit |
|--------------|-------|
| GET | 300 requests/minute |

## Endpoints

### Get Business Categories

Fetch the list of supported business categories for a specific country.

| | |
|---|---|
| **Method** | GET |
| **Endpoint** | `/manage/v1/business-categories/{country}` |

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| country | string | Yes | ISO 3 country code (exactly 3 characters) |

#### Supported Countries

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

#### Example Request

```bash
curl --request GET \
  --url https://api.brightlocal.com/manage/v1/business-categories/USA \
  --header 'Accept: application/json' \
  --header 'x-api-key: YOUR_API_KEY'
```

#### Example Response

```json
{
  "total_count": 1,
  "items": [
    {
      "id": 9661,
      "name": "Abbey"
    },
    {
      "id": 9662,
      "name": "Abortion Clinic"
    },
    {
      "id": 9663,
      "name": "Accountant"
    }
  ]
}
```

#### Response Fields

| Field | Type | Description |
|-------|------|-------------|
| total_count | integer | Total number of categories |
| items | array | List of category objects |
| items[].id | integer | Category ID (use for `business_category_id`) |
| items[].name | string | Category name |

#### Response Codes

| Code | Description |
|------|-------------|
| 200 | OK - Categories retrieved successfully |
| 400 | Bad Request - Invalid country code |
| 401 | Unauthorized - Invalid API key |
| 404 | Not Found - Country not supported |

## Usage with Locations API

When creating a location, use the category `id` from this API for the `business_category_id` field:

```bash
curl --request POST \
  --url https://api.brightlocal.com/manage/v1/locations \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: YOUR_API_KEY' \
  --data '{
    "business_name": "John Smith CPA",
    "business_category_id": 9663,
    "location_reference": "location-001",
    "country": "USA",
    "address": {
      "address1": "123 Main St",
      "region": "California",
      "city": "Los Angeles",
      "postcode": "90001"
    },
    "telephone": "+12135551234",
    "urls": {
      "website_url": "https://johnsmithcpa.com"
    }
  }'
```

## Additional Categories

You can assign up to 5 additional categories using the `extra_business_category_ids` field:

```json
{
  "business_category_id": 9663,
  "extra_business_category_ids": [9700, 9701, 9702]
}
```

## Best Practices

1. **Cache Categories**: Category lists don't change frequently. Cache the results to reduce API calls.

2. **Use Country-Specific Categories**: Always fetch categories for the same country as your location.

3. **Primary Category Selection**: Choose the most specific and accurate primary category for the business.

4. **Additional Categories**: Use `extra_business_category_ids` for secondary business activities.

## Common Categories Reference

Here are some commonly used category IDs (exact IDs may vary by country):

| Category Name | Typical Use |
|---------------|-------------|
| Restaurant | Food service establishments |
| Plumber | Plumbing services |
| Dentist | Dental practices |
| Lawyer | Legal services |
| Real Estate Agent | Property sales/rentals |
| Auto Repair Shop | Vehicle maintenance |
| Hair Salon | Hair styling services |
| Gym | Fitness centers |
| Hotel | Lodging |
| Retail Store | General retail |

**Note**: Always use the Get Business Categories endpoint to retrieve current, valid category IDs for your country.

## Related APIs

- [Locations API](locations.md) - Create and manage locations using category IDs
- [Clients API](clients.md) - Organize locations under clients
