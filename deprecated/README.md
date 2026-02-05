# Deprecated APIs

> **Warning**: The APIs documented in this folder have been deprecated and are no longer supported. Do not use these APIs for new integrations.

## Overview

BrightLocal has deprecated several APIs from the legacy documentation portal (apidocs.brightlocal.com) and replaced them with updated versions on the new developer portal (developer.brightlocal.com).

## Deprecated APIs List

| API | Replacement | New Documentation |
|-----|-------------|-------------------|
| [Rank Checker](./rank-checker.md) | Local Rank Tracker API | [brightlocal.stoplight.io](https://brightlocal.stoplight.io/docs/management-apis/zmd64hm2pgta3-local-rank-tracker) |
| [Citation Builder](./citation-builder.md) | Citation Builder API (New) | [developer.brightlocal.com](https://developer.brightlocal.com/docs/management-apis/agjq3z0pfs6nk-citation-builder) |
| [Reputation Manager](./reputation-manager.md) | Reputation Manager API (New) | [developer.brightlocal.com](https://developer.brightlocal.com/docs/management-apis/p7axgeo0csqf5-reputation-manager) |
| [Local Directories](./local-directories.md) | Listings API | [developer.brightlocal.com](https://developer.brightlocal.com/docs/data-apis/8owbgtne72ygc-listings-api) |
| [Rankings](./rankings.md) | Rankings API (New) | [developer.brightlocal.com](https://developer.brightlocal.com/docs/data-apis/a65f86e53ce22-rankings-api) |
| [Clients](./clients.md) | Clients API (New) | [developer.brightlocal.com](https://developer.brightlocal.com/docs/management-apis/j13dhiwfwfldc-clients) |
| [Locations](./locations.md) | Locations API (New) | [developer.brightlocal.com](https://developer.brightlocal.com/docs/management-apis/a8chirfprjb2p-locations) |

## Migration Guidance

When migrating from deprecated APIs:

1. **Review the new API documentation** - Each replacement API has updated endpoints, parameters, and response formats
2. **Update base URLs** - The new APIs use `developer.brightlocal.com` instead of `tools.brightlocal.com/seo-tools/api`
3. **Check authentication** - While still API key-based, authentication methods may have been updated
4. **Test thoroughly** - New APIs may return different response structures

## Legacy Base URL

The deprecated APIs used:
```
https://tools.brightlocal.com/seo-tools/api
```

## Support

For migration assistance, contact BrightLocal support.

## Source

This documentation was extracted from the legacy API documentation at [apidocs.brightlocal.com](https://apidocs.brightlocal.com/).
