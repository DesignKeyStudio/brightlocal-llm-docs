# Locations API

> **Status**: Current
> **Portal**: developer.brightlocal.com
> **Base URL**: `https://api.brightlocal.com`
> **Version**: v1.0

## Overview

The Locations API allows you to manage business location records in your BrightLocal account. Locations contain comprehensive business information including NAP (Name, Address, Phone), business categories, opening hours, social profiles, and more.

## Authentication

Include your API key in the `x-api-key` header:

```bash
curl --header 'x-api-key: YOUR_API_KEY'
```

## Rate Limits

| Request Type | Limit |
|--------------|-------|
| POST/PUT/DELETE | 100 requests/minute |
| GET | 300 requests/minute |

## Endpoints

### Create Location

Create a new location with comprehensive business information.

| | |
|---|---|
| **Method** | POST |
| **Endpoint** | `/manage/v1/locations` |

#### Request Body

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| business_name | string | Yes | Business name (<= 90 chars) |
| location_reference | string | Yes | Unique reference (letters, numbers, hyphens only, 1-50 chars) |
| country | string | Yes | ISO 3 country code (e.g., USA, GBR, CAN) |
| address | object | Yes | Address information |
| address.address1 | string | Yes | Primary street address (1-80 chars) |
| address.address2 | string | No | Secondary address (<= 80 chars) |
| address.region | string | Conditional | State/province (required for USA, GBR, CAN, AUS) |
| address.region_code | string | No | State/province code (2-3 chars, e.g., NY) |
| address.city | string | No | City name (<= 100 chars) |
| address.postcode | string | No | Postal/ZIP code (<= 80 chars) |
| telephone | string | Yes | Primary phone number (<= 20 chars) |
| business_category_id | integer | Yes | Primary business category ID (see Business Categories API) |
| urls | object | Yes | Business URLs |
| urls.website_url | string | Yes | Website URL (<= 256 chars) |
| urls.menu_url | string | No | Menu URL |
| urls.reservation_url | string | No | Reservation URL |
| urls.order_url | string | No | Online ordering URL |
| urls.gallery_url | string | No | Photo gallery URL |
| urls.event_url | string | No | Events URL |
| description | string | No | Business description (1-500 chars) |
| language | string | No | For Canada only: CAN:FR or CAN:EN |
| status | string | No | Location status |
| client_id | integer | No | Associate with a client |
| is_service_area_business | boolean | No | Service area business flag |
| contact | object | No | Contact information |
| opening_hours | object | No | Regular and special hours |
| extra_business_category_ids | array | No | Additional category IDs (<= 5 items) |
| geo_location | object | No | Latitude/longitude coordinates |
| payment_methods | array | No | Accepted payment methods |
| services_or_products | array | No | Services/products offered (<= 5 items) |
| licenses | object | No | License numbers |
| social_profiles | object | No | Social media URLs |
| timezone | string | No | Timezone offset (e.g., -05:00) |
| white_label_settings | object | No | White label configuration |

#### Contact Object

| Field | Type | Description |
|-------|------|-------------|
| first_name | string | Contact first name (<= 20 chars) |
| last_name | string | Contact last name (<= 20 chars) |
| mobile | string | Mobile phone (<= 20 chars) |
| telephone | string | Contact phone (<= 20 chars) |
| email | string | Contact email |
| fax | string | Fax number (<= 20 chars) |

#### Opening Hours Object

```json
{
  "regular": {
    "apply_to_all": true,
    "monday": {
      "status": "open",
      "hours": [{"start": "07:00", "end": "23:00"}]
    },
    "tuesday": {
      "status": "open",
      "hours": [{"start": "07:00", "end": "23:00"}]
    }
  },
  "special": {
    "days": [
      {
        "date": "2024.12.31",
        "status": "open",
        "hours": [{"start": "07:00", "end": "18:00"}]
      }
    ]
  },
  "reopen_date": "2024-01-15"
}
```

#### Social Profiles Object

| Field | Type | Description |
|-------|------|-------------|
| facebook_url | string | Facebook page URL |
| linkedin_url | string | LinkedIn profile URL |
| x_url | string | X (Twitter) profile URL |
| instagram_url | string | Instagram profile URL |
| pinterest_url | string | Pinterest profile URL |
| tiktok_url | string | TikTok profile URL |
| youtube_url | string | YouTube channel URL |

#### Payment Methods

Allowed values: `cash`, `visa`, `mastercard`, `amex`, `cheque`, `invoice`, `insurance`, `atm`, `travelers`, `financing`, `paypal`, `discover`

#### Example Request

```bash
curl --request POST \
  --url https://api.brightlocal.com/manage/v1/locations \
  --header 'Accept: application/json' \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: YOUR_API_KEY' \
  --data '{
    "business_name": "Alibaba Super Kebab",
    "location_reference": "Alibaba-Kebab-Reference-X-1",
    "country": "CAN",
    "address": {
      "address1": "900 William St.",
      "address2": "Suite 100",
      "region": "Ontario",
      "region_code": "ON",
      "city": "Toronto",
      "postcode": "M5V 1E3"
    },
    "telephone": "+14165551234",
    "business_category_id": 9661,
    "urls": {
      "website_url": "https://www.alibaba-kebab.ca",
      "menu_url": "https://www.alibaba-kebab.ca/menu"
    },
    "description": "Authentic Middle Eastern cuisine",
    "contact": {
      "first_name": "John",
      "last_name": "Smith",
      "email": "john@alibaba-kebab.ca"
    },
    "social_profiles": {
      "facebook_url": "https://www.facebook.com/alibabakebab",
      "instagram_url": "https://www.instagram.com/alibabakebab"
    },
    "payment_methods": ["cash", "visa", "mastercard"]
  }'
```

#### Example Response

```json
{
  "location_id": 1
}
```

#### Response Codes

| Code | Description |
|------|-------------|
| 201 | Created - Location created successfully |
| 400 | Bad Request - Invalid parameters |
| 401 | Unauthorized - Invalid API key |

---

### Get Location

Retrieve a specific location by ID.

| | |
|---|---|
| **Method** | GET |
| **Endpoint** | `/manage/v1/locations/{location_id}` |

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| location_id | integer | Yes | The location ID |

#### Example Request

```bash
curl --request GET \
  --url https://api.brightlocal.com/manage/v1/locations/123 \
  --header 'Accept: application/json' \
  --header 'x-api-key: YOUR_API_KEY'
```

---

### Find Locations

Search for locations in your account.

| | |
|---|---|
| **Method** | GET |
| **Endpoint** | `/manage/v1/locations` |

#### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | number | No | Page number for pagination |
| client_id | integer | No | Filter by client ID |
| location_reference | string | No | Filter by location reference |

---

### Update Location

Update an existing location.

| | |
|---|---|
| **Method** | PATCH |
| **Endpoint** | `/manage/v1/locations/{location_id}` |

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| location_id | integer | Yes | The location ID to update |

All fields from Create Location are available for update.

---

### Delete Location

Delete a location from your account.

| | |
|---|---|
| **Method** | DELETE |
| **Endpoint** | `/manage/v1/locations/{location_id}` |

---

### Upload Logo

Upload a logo image for a location.

| | |
|---|---|
| **Method** | POST |
| **Endpoint** | `/manage/v1/locations/{location_id}/logo` |

---

### Delete Logo

Remove the logo from a location.

| | |
|---|---|
| **Method** | DELETE |
| **Endpoint** | `/manage/v1/locations/{location_id}/logo` |

---

### Upload Photo

Upload a photo for a location.

| | |
|---|---|
| **Method** | POST |
| **Endpoint** | `/manage/v1/locations/{location_id}/photos` |

---

### Delete Photo

Remove a photo from a location.

| | |
|---|---|
| **Method** | DELETE |
| **Endpoint** | `/manage/v1/locations/{location_id}/photos/{photo_id}` |

---

### Create Location from Place ID

Create a location using a Google Place ID.

| | |
|---|---|
| **Method** | POST |
| **Endpoint** | `/manage/v1/locations/place-id` |

---

### Create Location from Maps URL

Create a location using a Google Maps URL.

| | |
|---|---|
| **Method** | POST |
| **Endpoint** | `/manage/v1/locations/maps-url` |

---

### Active Sync Change Alerts

Get change alerts for Active Sync locations.

| | |
|---|---|
| **Method** | GET |
| **Endpoint** | `/manage/v1/locations/{location_id}/active-sync/alerts` |

---

### Cancel Active Sync Request

Cancel an Active Sync request.

| | |
|---|---|
| **Method** | POST |
| **Endpoint** | `/manage/v1/locations/{location_id}/active-sync/cancel` |

## Supported Countries

| Code | Country | Region Required |
|------|---------|-----------------|
| USA | United States | Yes |
| GBR | United Kingdom | Yes |
| CAN | Canada | Yes |
| AUS | Australia | Yes |
| DEU | Germany | No |
| IRL | Ireland | No |
| NLD | Netherlands | No |
| NZL | New Zealand | No |
| PHL | Philippines | No |
| SGP | Singapore | No |
| HKG | Hong Kong | No |
| MAC | Macau | No |
| TWN | Taiwan | No |
| ZAF | South Africa | No |

## Best Practices

1. **Use Unique References**: Set `location_reference` to your internal location ID for easy mapping

2. **Get Category IDs First**: Use the Business Categories API to get valid `business_category_id` values

3. **Provide Complete Data**: Include as much information as possible for accurate listing management

4. **Link to Clients**: Use `client_id` to organize locations under clients

5. **Consistent Formatting**: Use consistent phone number and address formatting across locations

## Related APIs

- [Clients API](clients.md) - Manage client records
- [Business Categories API](business-categories.md) - Get business category IDs
