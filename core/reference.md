# API Reference Data

> **Status**: Current

This document contains reference data for the BrightLocal API including supported countries, directories, error codes, and other lookup values.

## Supported Countries

The following countries are supported across BrightLocal APIs:

| Country | ISO Code |
|---------|----------|
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

**Note**: Some APIs may only support a subset of these countries. Check individual API documentation for specific support.

## Supported Review Directories

The following directories are supported for review fetching via the Reviews API:

### Major Platforms

| Directory | Pagination Method | Items Per Page |
|-----------|-------------------|----------------|
| Google | Token-based | Variable |
| Yelp | Token-based | Variable |
| Facebook | Token-based | Variable |
| TripAdvisor | Page-based | 5-10 reviews |
| Trustpilot | Page-based | 20 reviews |
| Yellow Pages | Page-based | 20 reviews |

### Industry-Specific Platforms

| Directory | Pagination Method | Items Per Page |
|-----------|-------------------|----------------|
| Dentist1800 | Page-based | 20 reviews |
| Expedia | Page-based | 500 reviews |
| Grubhub | Page-based | 35 reviews |
| Healthgrades | Page-based | Variable |
| Insiderpages | Page-based | 50 reviews |
| OpenTable | Page-based | 40 reviews |
| RealSelf | Page-based | 10 reviews |
| SelfStorage | Page-based | 3 reviews |

### Additional Supported Directories

BrightLocal supports over 80 review sites including:

- bbb (Better Business Bureau)
- citysearch
- foursquare
- angieslist
- avvo
- cars.com
- carfax
- dealerrater
- homeadvisor
- houzz
- lawyers.com
- martindale
- realtor.com
- vitals
- webmd
- zocdoc
- zillow

**Note**: The complete list of supported directories may vary. Contact BrightLocal support for the full current list.

## Error Codes

### Common Authentication Errors

| Error Code | Description |
|------------|-------------|
| INVALID_API_KEY | API key is missing, invalid, or lacks required permissions |

### Batch API Errors

| Error Code | Description |
|------------|-------------|
| INVALID_BATCH_ID | Batch ID not found or invalid |
| BATCH_ALREADY_COMMITTED | Cannot add jobs to a committed batch |
| BATCH_NO_JOBS | Batch cannot be committed with no jobs |

### Reviews API Errors

| Error Code | Description |
|------------|-------------|
| INVALID_PROFILE_URL | Profile URL is not valid or recognized |
| INVALID_COUNTRY | Country code not supported |
| INVALID_LOCAL_DIRECTORY | Directory identifier not recognized |
| INVALID_DATE_FROM | Date format invalid (use Y-m-d or Y-m-d H:i:s) |
| INVALID_REVIEWS_LIMIT | Limit must be positive number or 'all' |
| INVALID_METHOD | Wrong HTTP method used (e.g., GET instead of POST) |

### Business Data Errors

| Error Code | Description |
|------------|-------------|
| INVALID_BUSINESS_NAMES | Business names parameter missing or invalid |
| INVALID_CITY | City parameter missing or invalid |
| INVALID_POSTCODE | Postcode parameter missing or invalid |

### Report API Errors

| Error Code | Description |
|------------|-------------|
| INVALID_REPORT_ID | Report ID not found |
| INVALID_LOCATION_ID | Location ID not found in account |
| NO_CREDITS | Insufficient account credits to run report |
| REPORT_RUNNING | Report is already in progress |

### Google Business Profile API Errors

| Error Code | Description |
|------------|-------------|
| INVALID_SCHEDULE | Schedule must be "Adhoc" or "Monthly" |
| INVALID_DAY_OF_MONTH | Day must be 1-28 or -1 (last day) |
| INVALID_REPORT_TYPE | Report type must be "with" or "without" |
| INVALID_SEARCH_TERMS | Search terms required (1-5 terms) |
| INVALID_GOOGLE_LOCATION | Location string not recognized by Google |

## Batch States

| State | Description |
|-------|-------------|
| Created | Batch created, jobs can be added |
| Committed | Closed for additions, processing pending |
| Running | Jobs are being processed |
| Stopped | Processing halted due to error |
| Finished | All jobs completed |

## Job States

| State | Description |
|-------|-------------|
| Pending | Job added but not yet queued |
| Queued | Job queued for processing |
| Processing | Job currently executing |
| Failed | Job execution failed |
| Completed | Job finished successfully, results available |

## Report Schedules

For scheduled reports (Citation Tracker, GBP):

### Monthly Schedule

| Parameter | Value | Description |
|-----------|-------|-------------|
| day_of_month | 1-28 | Specific day of month |
| day_of_month | -1 | Last day of month |
| day_of_month | 0 | Disabled |

### Weekly Schedule (Citation Tracker)

| Parameter | Value | Description |
|-----------|-------|-------------|
| weekly-run | 1 | Monday |
| weekly-run | 2 | Tuesday |
| weekly-run | 3 | Wednesday |
| weekly-run | 4 | Thursday |
| weekly-run | 5 | Friday |
| weekly-run | 6 | Saturday |
| weekly-run | 7 | Sunday |
| weekly-run | 0 | Disabled |

## Sort Options

For Reviews API:

| Value | Description |
|-------|-------------|
| date | Sort by review date (default) |
| rating | Sort by star rating |

## Notification Settings

| Value | Description |
|-------|-------------|
| Yes / yes | Enable email notifications |
| No / no | Disable email notifications (default) |

**Note**: Maximum 5 email addresses can be specified per report.

## Date Formats

| Format | Example | APIs |
|--------|---------|------|
| Y-m-d | 2024-07-30 | Reviews (date-from) |
| Y-m-d H:i:s | 2024-07-30 14:30:00 | Reviews (date-from) |

## Review Limits

| Value | Description |
|-------|-------------|
| Positive integer | Specific number of reviews |
| 'all' | Retrieve all available reviews |
| Default | 100 reviews |

**Maximum**: 500 reviews per single request. Use pagination for more.

## Domain Authority Scale

Domain authority scores returned by Offsite SEO and Citation Tracker APIs use a 0-100 scale:

| Range | Interpretation |
|-------|----------------|
| 0-20 | Low authority |
| 21-40 | Below average |
| 41-60 | Average |
| 61-80 | Above average |
| 81-100 | High authority |

## HTTP Status Codes

| Code | Meaning |
|------|---------|
| 200 | OK - Request successful |
| 201 | Created - Resource created successfully |
| 400 | Bad Request - Invalid parameters |
| 401 | Unauthorized - Invalid API key |
| 404 | Not Found - Resource not found |
| 405 | Method Not Allowed - Wrong HTTP method |
| 500 | Server Error - Internal server error |
