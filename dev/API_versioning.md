---
title: "API versions"
description: "Learn about Cognite API versioning. "
---

Cognite's stable REST APIs are prefixed with `v1`. This version prefix is a long-term solution, and there are no plans to increase it in the near future.

Within the `v1` version, we use a date-based versioning scheme. Pass a date in the YYYYMMDD format to get API versions released on that date or earlier. Then, pass the desired calendar version to the API using the `Cdf-Version` header.

This version header retrieves the API versions released on February 5, 2023, and earlier:

```
Cdf-Version: 20230205
```

Any requests without a calendar version header will default to the date version `20230101`. This ensures that already deployed clients, applications, and extractors that don't request a specific version will continue to function as we roll out new breaking changes. Non-breaking changes will be available to all API clients. Breaking changes require each client to be upgraded to request newer versions.

We can publish new changes to existing calendar versions, but these will always be [backwards-compatible](#backwards-compatible-non-breaking-changes). If we have to change the API in a [backwards-incompatible](#backwards-incompatible-breaking-changes) way, we will release those changes as a new calendar version.

Stable API versions are supported by JavaScript SDK 2.x.x and Python SDK 1.x.x or newer.

After publishing a new calendar version, we can mark the older calendar versions as _deprecated_.

<Note>
  Older calendar versions will be available for 12 months after being marked as deprecated.
</Note>

## Beta versions

To request a beta version from the REST API, use the `Cdf-Version` header with a date and the suffix `-beta`.

This version header retrieves the **beta** API versions released on February 5, 2023, and earlier:

```
Cdf-Version: 20230205-beta
```

## Backwards compatibility

If we change the current stable API in a way that's backward-incompatible, we'll release the changes as a new calendar version.

A backwards-incompatible change is a change that causes a previously successful request or successful parsing of the response to fail or give different results after the change is released.

### Backwards-compatible (non-breaking) changes

The following are examples of backwards-compatible (non-breaking) changes:

- Increasing the request limits.
- Adding properties to the response body.
- Adding properties to the request body (as long as it doesn't affect earlier requests).
- Adding support for more query parameters (as long as it doesn't affect earlier requests).

### Backwards-incompatible (breaking) changes

The following are examples of backwards-incompatible (breaking) changes:

- Removing an endpoint.
- Removing a property from the response.
- Changing the error response format (not the status code).
- Changing the status code from 2xx to another 2xx.
- Reducing the limit on a request (not the response). Example: the number of assets to retrieve by `assetIds`.
- Changing the name of query parameters and body properties.
- Changing the sort order if the user expected the original order.