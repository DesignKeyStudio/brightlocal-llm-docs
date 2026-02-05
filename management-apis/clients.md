# Clients API

> **Status**: Current
> **Portal**: developer.brightlocal.com
> **Base URL**: `https://api.brightlocal.com`
> **Version**: v1.0

## Overview

The Clients API allows you to manage client records in your BrightLocal account. Clients serve as organizational containers for grouping locations and reports.

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

### Get Client

Retrieve a specific client by ID.

| | |
|---|---|
| **Method** | GET |
| **Endpoint** | `/manage/v1/clients/{client_id}` |

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| client_id | integer | Yes | The client ID |

#### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | number | No | Page number for pagination |

#### Example Request

```bash
curl --request GET \
  --url https://api.brightlocal.com/manage/v1/clients/123 \
  --header 'Accept: application/json' \
  --header 'x-api-key: YOUR_API_KEY'
```

#### Example Response

```json
{
  "client_id": 1,
  "name": "Test Client",
  "type": "client",
  "unique_reference": "Test-Client-TC1",
  "website_url": "www.test-client.com"
}
```

#### Response Codes

| Code | Description |
|------|-------------|
| 200 | OK - Client retrieved successfully |
| 400 | Bad Request - Invalid parameters |
| 401 | Unauthorized - Invalid API key |
| 404 | Not Found - Client does not exist |

---

### Find Clients

Search for clients in your account.

| | |
|---|---|
| **Method** | GET |
| **Endpoint** | `/manage/v1/clients` |

#### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| page | number | No | Page number for pagination |
| name | string | No | Filter by client name |

#### Example Request

```bash
curl --request GET \
  --url https://api.brightlocal.com/manage/v1/clients \
  --header 'Accept: application/json' \
  --header 'x-api-key: YOUR_API_KEY'
```

---

### Create Client

Create a new client record.

| | |
|---|---|
| **Method** | POST |
| **Endpoint** | `/manage/v1/clients` |

#### Request Body

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| name | string | Yes | Client name |
| website_url | string | Yes | Client's website URL |
| unique_reference | string | No | Your custom reference ID |
| type | string | No | Client type: `client`, `lead`, or `na` |

#### Example Request

```bash
curl --request POST \
  --url https://api.brightlocal.com/manage/v1/clients \
  --header 'Accept: application/json' \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: YOUR_API_KEY' \
  --data '{
    "name": "Client name",
    "website_url": "www.client.com",
    "unique_reference": "Client-Ref-1",
    "type": "client"
  }'
```

#### Example Response

```json
{
  "client_id": 1
}
```

#### Response Codes

| Code | Description |
|------|-------------|
| 201 | Created - Client created successfully |
| 400 | Bad Request - Invalid parameters |
| 401 | Unauthorized - Invalid API key |

---

### Update Client

Update an existing client.

| | |
|---|---|
| **Method** | PATCH |
| **Endpoint** | `/manage/v1/clients/{client_id}` |

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| client_id | integer | Yes | The client ID to update |

#### Request Body

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| name | string | No | Updated client name |
| website_url | string | No | Updated website URL |
| unique_reference | string | No | Updated custom reference |
| type | string | No | Updated type: `client`, `lead`, or `na` |

#### Example Request

```bash
curl --request PATCH \
  --url https://api.brightlocal.com/manage/v1/clients/123 \
  --header 'Accept: application/json' \
  --header 'Content-Type: application/json' \
  --header 'x-api-key: YOUR_API_KEY' \
  --data '{
    "name": "Updated Client Name",
    "type": "client"
  }'
```

---

### Delete Client

Delete a client from your account.

| | |
|---|---|
| **Method** | DELETE |
| **Endpoint** | `/manage/v1/clients/{client_id}` |

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| client_id | integer | Yes | The client ID to delete |

#### Example Request

```bash
curl --request DELETE \
  --url https://api.brightlocal.com/manage/v1/clients/123 \
  --header 'Accept: application/json' \
  --header 'x-api-key: YOUR_API_KEY'
```

#### Response Codes

| Code | Description |
|------|-------------|
| 200 | OK - Client deleted successfully |
| 400 | Bad Request - Invalid parameters |
| 401 | Unauthorized - Invalid API key |
| 404 | Not Found - Client does not exist |

## Client Types

| Type | Description |
|------|-------------|
| client | Active client |
| lead | Prospective client/lead |
| na | Not applicable/other |

## Client Object

| Field | Type | Description |
|-------|------|-------------|
| client_id | number | Unique client identifier |
| name | string | Client name |
| type | string | Client type (client, lead, na) |
| unique_reference | string | Your custom reference ID |
| website_url | string | Client's website URL |

## Best Practices

1. **Use Unique References**: Set `unique_reference` to your internal client ID for easy mapping between systems

2. **Organize with Client Types**: Use the `type` field to distinguish between active clients, leads, and other records

3. **Link Locations**: After creating a client, use the `client_id` when creating locations to group them under this client

## Related APIs

- [Locations API](locations.md) - Manage locations associated with clients
- [Business Categories API](business-categories.md) - Get category IDs for locations
