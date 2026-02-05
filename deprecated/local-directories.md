# Local Directories API

> **Status**: DEPRECATED
>
> **Deprecation Notice**: This API has been deprecated and is no longer supported.
>
> **Replacement**: [Listings API](https://developer.brightlocal.com/docs/data-apis/8owbgtne72ygc-listings-api)

## Overview

The Local Directories API provided raw data retrieval capabilities for fetching reviews and business information from various local directory sites. This functionality has been migrated to the new Listings API.

## Authentication

- **Method**: API key authentication
- **Parameter**: `api-key`
- **Base URL**: `https://tools.brightlocal.com/seo-tools/api`

## Legacy Endpoints

### Fetch Reviews (by Profile URL)

**`POST /v4/ld/fetch-reviews`**

This was a batch method for fetching reviews using a direct profile URL.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| batch-id | string | Yes | Batch container ID |
| profile-url | string | Yes | Directory profile URL (use Google Link & ID Generator for Google profiles) |
| country | string | Yes | Country code (see supported countries) |
| sort | string | No | Sort order: 'rating' or 'date' (default: 'date') |
| reviews-limit | string/number | No | Number of reviews or 'all' (default: 100) |
| date-from | string | No | Filter by date (format: Y-m-d or Y-m-d H:i:s) |
| start-page | string/integer | No | Starting page for pagination |

#### Legacy Response Example (201 Created)

```json
{
  "success": true,
  "job-id": 318
}
```

---

### Fetch Reviews (by Business Data)

**`POST /v4/ld/fetch-reviews-by-business-data`**

This was a batch method for fetching reviews using business information.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| batch-id | string | Yes | Batch container ID |
| business-names | string | Yes | Business name(s), newline-separated for multiple |
| city | string | Yes | City name |
| postcode | string | Yes | ZIP/postal code |
| local-directory | string | Yes | Directory identifier (see supported directories) |
| country | string | Yes | Country code (USA only for this endpoint) |
| street-address | string | No | Street address |
| telephone | string | No | Phone number |
| sort | string | No | Sort order: 'rating' or 'date' |
| reviews-limit | string/number | No | Number of reviews (default: 100) |
| date-from | string | No | Filter by date |
| start-page | string/integer | No | Starting page for pagination |

#### Legacy Response Example (201 Created)

```json
{
  "success": true,
  "job-id": 318
}
```

## Supported Directories

The following directories were supported with their pagination details:

| Directory | Reviews Per Page/Token |
|-----------|----------------------|
| google | Token-based pagination |
| trustpilot | 20 |
| dentist1800 | 20 |
| expedia | 500 |
| facebook | Token-based pagination |
| grubhub | 35 |
| healthgrades | Varies |
| insiderpages | 50 |
| opentable | 40 |
| realself | 10 |
| selfstorage | 3 |
| tripadvisor | 5 or 10 |
| yellowpages | 20 |

## Supported Countries

| Country | Code |
|---------|------|
| Australia | AUS |
| Canada | CAN |
| Germany | DEU |
| Hong Kong | HKG |
| Ireland | IRL |
| Macau | MAC |
| Netherlands | NLD |
| New Zealand | NZL |
| Philippines | PHL |
| Singapore | SGP |
| South Africa | ZAF |
| Taiwan | TWN |
| United Kingdom | GBR |
| United States | USA |

## Pagination

The API supported fetching up to 500 reviews per request. For older reviews, use the `start-page` parameter. Responses included a `next-start-page` value for subsequent requests.

## Migration Guide

All users of the Local Directories API should migrate to the new Listings API, which provides:

- Updated endpoint structure
- Enhanced directory coverage
- Improved data accuracy
- Better integration with BrightLocal's platform

### Key Differences

| Feature | Old Local Directories API | New Listings API |
|---------|--------------------------|------------------|
| Base URL | tools.brightlocal.com/seo-tools/api | developer.brightlocal.com |
| Documentation | apidocs.brightlocal.com | developer.brightlocal.com |
| Method Type | Batch-based | Direct requests |

## Error Codes

| Error Code | Description |
|------------|-------------|
| INVALID_API_KEY | API key is missing or invalid |
| INVALID_BATCH_ID | Batch ID not found |
| INVALID_PROFILE_URL | Profile URL is missing or invalid |
| INVALID_COUNTRY | Country code not supported |
| INVALID_LOCAL_DIRECTORY | Directory not supported |

## Support

For assistance migrating to the new Listings API, contact BrightLocal support or refer to the [new API documentation](https://developer.brightlocal.com/docs/data-apis/8owbgtne72ygc-listings-api).
