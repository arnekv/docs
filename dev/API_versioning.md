---
title: "API versions"
description: "Learn about Cognite API versioning, the policy for backwards compatibility, and the end-of-life schedule for the APIs"
---

Cognite's stable REST APIs are prefixed with `v1`. This version prefix is a long-term solution, and there are no plans to increase it in the near future.

Within the `v1` version, we use a date-based versioning scheme. Pass a date in the YYYYMMDD format to get API versions released on that date or earlier. Then, pass the desired calendar version to the API using the `Cdf-Version` header.

This version header retrieves the API versions released on February 5, 2023, and earlier:

```
Cdf-Version: 20230205
```

Any requests without a calendar version header will default to the date version `20230101`. This ensures that already deployed clients, applications, and extractors that don't request a specific version will continue to function as we roll out new breaking changes. Non-breaking changes will be available to all clients of the API, while breaking changes require each client to be upgraded to request newer versions.

We can publish new changes to existing calendar versions, but these will always be [backwards-compatible](#backwards-compatible-non-breaking-changes). If we have to change the API in a [backwards-incompatible](#backwards-incompatible-breaking-changes) way, we will release those changes as a new calendar version.

Stable API versions are supported by JavaScript SDK 2.x.x and Python SDK 1.x.x or newer.

After publishing a new calendar version, we can mark the older calendar versions as _deprecated_.

:<warning>
Older calendar versions will be **available for 12 months after being marked deprecated** to provide a migration period for the users.
Extension to this support period can be granted in special cases. Please contact [support@cognite.com](mailto:support@cognite.com).
</warning>

## Beta versions

:::caution NOTE
We do not recommend beta versions for production environments.
:::

The beta versions provide an early peek into the latest features that will soon be available in the stable versions.
Every time a new feature is released, you can develop solutions early and be ready for deployment.
We typically promote beta features to the stable version within three months.

To request a beta version from the REST API, use the `Cdf-Version` header with a date and the suffix `-beta`.

For example: The below version header will request the beta API versions released on the 5th of February, 2023 or earlier.

```
Cdf-Version: 20230205-beta
```

We can publish new changes to the existing beta calendar versions, but these will always be [backwards-compatible](#backwards-compatible-non-breaking-changes).

> Beta features are supported by the JavaScript Beta SDK 3.x.x and Python SDK 2.x.x(using the Beta client).

## Playground (deprecated)

:::warning Warning
Never use playground in production environments.
:::

We have stopped publishing the documentation for Playground API and will gradually remove the API endpoints.
Most of the Playground APIs were removed by 2023-06-01, and the remaining will be removed subsequently.

If you have any questions or issues moving to API v1 and the latest date version, contact [support@cognite.com](mailto:support@cognite.com).

## Older API versions (removed)

The following API versions have been removed. If you make direct API calls to these versions or use SDKs or other tools that depend on them, you'll receive errors.

| API version | SDK version                                                       | Removed            |
| ----------- | ----------------------------------------------------------------- | ------------------ |
| API 0.5     | Python SDK 0.x.x or earlier<p />  JavaScript SDK 1.x.x or earlier | March 22, 2021     |
| API 0.6     | Python SDK 0.x.x or earlier<p />  JavaScript SDK 1.x.x or earlier | April 1, 2020      |
| API 0.4     | Python SDK 0.11.x or earlier                                      | September 10, 2019 |
| API 0.3     | Python SDK 0.11.x or earlier                                      | September 10, 2019 |

## Backwards compatibility

If we change the current stable API in a backwards-incompatible way, we'll release those changes as a new calendar version.

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