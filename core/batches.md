# Batches API

> **Status**: Current

## Overview

The Batches API serves as a container for grouping multiple API requests together, enabling batch processing of data retrieval operations. Most BrightLocal raw data APIs (Reviews, Offsite SEO, etc.) require requests to be submitted within a batch context.

**Key Concept**: Create a batch, add jobs to it, commit it to start processing, then poll for results.

## Authentication

- API key required via `api-key` parameter
- Base URL: `https://tools.brightlocal.com/seo-tools/api`

## Batch Workflow

```
1. Create Batch → Get batch-id
2. Add Jobs → Submit API requests to the batch
3. Commit Batch → Signal completion, start processing
4. Poll Results → Check status until "Finished"
5. Delete Batch → Clean up when done
```

## Batch States

| State | Description |
|-------|-------------|
| Created | Batch created, jobs can be added |
| Committed | Closed for additions, awaiting processing |
| Running | Jobs are being processed |
| Stopped | One or more jobs failed |
| Finished | All processing complete |

## Job States

| State | Description |
|-------|-------------|
| Pending | Job added, not yet queued |
| Queued | Job queued for processing |
| Processing | Job currently executing |
| Failed | Job execution failed |
| Completed | Job finished successfully |

## Endpoints

### Create Batch

**`POST /v4/batch`**

Creates a new batch container for grouping API requests.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| stop-on-job-error | integer | No | Stop batch if a job fails (0 or 1, default: 0) |
| callback | string | No | URL for completion notifications |
| callback_request_format | string | No | Callback format: 'form' or 'json' (default: 'form') |

#### Request Example

```bash
curl -X POST "https://tools.brightlocal.com/seo-tools/api/v4/batch" \
  -d "api-key=YOUR_API_KEY" \
  -d "stop-on-job-error=0"
```

#### Response Example (201 Created)

```json
{
  "success": true,
  "batch-id": "17"
}
```

---

### Commit Batch

**`PUT /v4/batch`**

Signals that all jobs have been added and processing should begin.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| batch-id | integer | Yes | Batch ID from create response |

#### Request Example

```bash
curl -X PUT "https://tools.brightlocal.com/seo-tools/api/v4/batch" \
  -d "api-key=YOUR_API_KEY" \
  -d "batch-id=17"
```

#### Response Example (200 OK)

```json
{
  "success": true
}
```

---

### Get Results

**`GET /v4/batch`**

Retrieves current batch status and any available results.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| batch-id | integer | Yes | Batch ID to check |

#### Request Example

```bash
curl -X GET "https://tools.brightlocal.com/seo-tools/api/v4/batch?api-key=YOUR_API_KEY&batch-id=17"
```

#### Response Example (200 OK)

```json
{
  "success": true,
  "status": "Finished",
  "results": {
    "job-id-1": { ... },
    "job-id-2": { ... }
  },
  "statuses": {
    "Completed": 2
  }
}
```

#### Status Values

- `Created` - Jobs can still be added
- `Committed` - Processing pending
- `Running` - Jobs being processed
- `Stopped` - Processing halted due to error
- `Finished` - All jobs complete

---

### Delete Batch

**`DELETE /v4/batch`**

Permanently removes a batch and all associated data.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| batch-id | integer | Yes | Batch ID to delete |

#### Request Example

```bash
curl -X DELETE "https://tools.brightlocal.com/seo-tools/api/v4/batch" \
  -d "api-key=YOUR_API_KEY" \
  -d "batch-id=17"
```

#### Response Example (200 OK)

```json
{
  "success": true
}
```

> **Warning**: This action cannot be undone. All retrieved data will be permanently deleted.

---

### Stop Batch

**`PUT /v4/batch/stop`**

Cancels batch processing mid-execution.

#### Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| api-key | string | Yes | Your API key |
| batch-id | integer | Yes | Batch ID to stop |

#### Request Example

```bash
curl -X PUT "https://tools.brightlocal.com/seo-tools/api/v4/batch/stop" \
  -d "api-key=YOUR_API_KEY" \
  -d "batch-id=17"
```

#### Response Example (200 OK)

```json
{
  "success": true
}
```

## Error Codes

| Error Code | Description |
|------------|-------------|
| INVALID_API_KEY | API key is missing or invalid |
| INVALID_BATCH_ID | Batch ID not found or invalid |
| BATCH_ALREADY_COMMITTED | Cannot add jobs to committed batch |

## Best Practices

1. **Batch Size**: Submit jobs in batches of hundreds for optimal performance
2. **Polling Interval**: Poll results every 5 seconds after commitment
3. **Cleanup**: Always delete batches when done to free resources
4. **Error Handling**: Check `stop-on-job-error` for critical workflows

## Notes

- Results accumulate progressively as jobs complete
- Use callbacks for async notification instead of polling
- All responses are JSON-encoded
