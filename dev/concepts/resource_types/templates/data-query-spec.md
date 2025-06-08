---
pagination_next: null
pagination_prev: null
title: Data querying
---

# Query template instances


:::caution Deprecation
Templates is being superseded with the released data modeling capabilities. See [Data modeling](https://docs.cognite.com/cdf/data_modeling/) for more information.
:::

You can query the template instances by using [GraphQL](https://graphql.org/learn/). A query has these key sections:

- A **query intent** (`query MyQueryName {`, or just `{` as a shorthand.
- The **query to run**. For templates this is usually `<type>Query`, for example `countryQuery` with an optional `alias`, `myAlias: countryQuery`)
- The **query parameters**. For example, `filter: {...}` or `limit: 1000`.
- The **fields to return** at a specified depth.

### GraphQL example

```graphql
query myQuery {
  countryQuery {
    items {
      name
      demographics {
        populationSize
        growthRate
      }
      deaths {
        datapoints(limit: 100) {
          timestamp
          value
        }
      }
    }
    nextCursor
  }
  demographicsQuery(
    filter: {
      _and: [{ populationSize: { gte: 2 } }, { populationSize: { lte: 10 } }]
    }
  ) {
    items {
      populationSize
      growthRate
      metadata {
        key
        value
      }
    }
  }
}
```

Where:

- `myQuery` declares the **query intent**.
- The **queries to run** are `countryQuery` and `demographicsQuery`.
- The **parameters** are `Country`:`100` for the `death` time series data points, and the `Demographics`: filter of `populationSize` between 2 and 10.
- The **fields to return**, for `Country`: `items` -> `name`, `demographic` -> `populationSize`, `growthRate`, etc.

For more information, see the [GraphQL documentation](https://graphql.org/learn/).

## Fields and parameters for queries

For `<type>Query` you can use these fields and parameters:

- **Fields**

  - `items` - the actual items to be returned.
  - `nextCursor` - the next set of items that can be returned. You specify this for the next query.

- **Parameters**

  - `filter` - the filter you want to provide based on the `fields` in your data model.
  - `cursor` - the `nextCursor` from the previous query.
  - `limit` - the maximum number of items to return. To fetch more items, use a anew query using the `nextCursor` parameter (**Default**: 1000).

### Filter types

|                   | Field               | Filter       | How to use                            |
| ----------------- | ------------------- | ------------ | ------------------------------------- |
| Top level filters | Any                 | `_and`       | `{_and: [{...},{...}]}`               |
|                   |                     | `_or`        | `{_or: [{...},{...}]}`                |
| Primitives        | String              | `eq`         | `{eq: "..."}`                         |
|                   |                     | `anyOfTerms` | `{anyOfTerms: ["..."]}`               |
|                   | Long/Int/Float      | `eq`         | `{eq: ...}`                           |
|                   |                     | `lt`         | `{lt: ...}` = less than               |
|                   |                     | `gt`         | `{gt: ...}` = greater than            |
|                   |                     | `lte`        | `{lte: ...}` = less than or equals    |
|                   |                     | `gte`        | `{gte: ...}` = greater than or equals |
|                   | Boolean             | N/A          | N/A                                   |
| CDF resources     | Asset               | N/A          | N/A                                   |
|                   | TimeSeries          | N/A          | N/A                                   |
|                   | SyntheticTimeSeries | N/A          | N/A                                   |
|                   | Sequence            | N/A          | N/A                                   |
|                   | File                | N/A          | N/A                                   |
|                   | Event               | N/A          | N/A                                   |

## Query fields for CDF types

The sections below, describe the fields you can specify under `type` when querying built-in CDF types.

For example, `Asset` -> `MetadataItem` has the `key` and `value` fields that you can specify in your query:

```graphql
# ...
  asset {
    metadata{
      key
      value
    }
  }
# ...
```

### Common for all resource types

```graphql
type MetadataItem {
  key: String
  value: String
}
```

### Time series

```graphql
type DatapointString implements Datapoint {
  timestamp: Long!
  value: Float
  stringValue: String
}

type DatapointFloat implements Datapoint {
  timestamp: Long!
  value: Float
}

type DatapointInt {
  timestamp: Long!
  value: Int
}

type AggregationResult {
  average: [DatapointFloat]
  max: [DatapointFloat]
  min: [DatapointFloat]
  count: [DatapointInt]
  sum: [DatapointFloat]
  interpolation: [DatapointFloat]
  stepInterpolation: [DatapointFloat]
  continuousVariance: [DatapointFloat]
  discreteVariance: [DatapointFloat]
  totalVariation: [DatapointFloat]
}

type TimeSeries {
  id: Long
  name: String
  metadata: [MetadataItem]
  description: String
  externalId: String
  isString: Boolean
  isStep: Boolean
  unit: String
  datapoints(start: Long, end: Long, limit: Int! = 100): [Datapoint]
  aggregatedDatapoints(
    start: Long
    end: Long
    limit: Int! = 100
    granularity: String!
  ): AggregationResult
  latestDatapoint: Datapoint
}
```

### Synthetic time series

```graphql
type SyntheticTimeSeries {
  name: String
  metadata: [MetadataItem]
  description: String
  isStep: Boolean
  unit: String
  datapointsWithGranularity(
    start: Long
    end: Long
    limit: Int! = 100
    granularity: String
  ): [Datapoint]
  datapoints(start: Long, end: Long, limit: Int! = 100): [Datapoint]
}
```

### Assets

```graphql
type Asset {
  id: Long!
  externalId: String
  name: String!
  description: String
  root: Asset
  parent: Asset
  source: String
  metadata: [MetadataItem]
}
```

### Files

```graphql
type File {
  id: Long!
  externalId: String
  name: String!
  directory: String
  mimeType: String
  source: String
  metadata: [MetadataItem]
  dataSetId: Long
  assets: [Asset]
  sourceCreatedTime: Long
  sourceModifiedTime: Long
  uploaded: Boolean!
  uploadedTime: Long
  createdTime: Long!
  lastUpdatedTime: Long!
  downloadUrl: String
}
```

### Sequences

```graphql
type Sequence {
  id: Long!
  name: String
  description: String
  assetId: String
  externalId: String
  metadata: [MetadataItem]
  dataSetId: Long
  createdTime: Long!
  lastUpdatedTime: Long!
}
```

## SDK documentation

- [Cognite Python SDK](https://cognite-sdk-python.readthedocs-hosted.com/en/latest/deprecated.html)
- [Cognite JavaScript SDK (Beta)](https://cognitedata.github.io/cognite-sdk-js/beta/classes/_stable_src_api_templates_templategraphqlapi_.templategraphqlapi.html)
