---
pagination_next: null
pagination_prev: null
---

# Pagination

Most resource types in Cognite Data Fusion (CDF) can be paginated, as indicated by the `nextCursor` field in the response. Pass the value of `nextCursor` as the cursor to get the next page of limit results. Note that all parameters except `cursor` have to stay the same.

A request can return fewer results than its limits, as long as there is a `nextCursor` that indicates that there are more data to fetch.

## Simple cursor example

```http
HTTP GET /assets?limit=1
```

```json
{
  "nextCursor": "8ZiApWzGe5RnTAE1N5SABLDNv7GKkUGiVUyUjzNsDvM",
  "items": [
    {
      "name": "23-TE-96116-04",
      "parentId": 3117826349444493,
      "description": "VRD - PH 1STSTGGEAR THRUST BRG OUT",
      "metadata": {
        "ELC_STATUS_ID": "1211",
        "RES_ID": "525283"
      },
      "id": 702630644612,
      "createdTime": 0,
      "lastUpdatedTime": 0,
      "rootId": 6687602007296940
    }
  ]
}
```

Getting the next page of assets.

:::info note
When you send more parameters to `/assets`, make sure that you also include them in all further requests in addition to the `cursor` parameter.
:::

```http
HTTP GET /assets?limit=1&cursor=8ZiApWzGe5RnTAE1N5SABLDNv7GKkUGiVUyUjzNsDvM
```

## Parallel retrieval

**Parallel retrieval** is a technique used to improve data retrieval performance in cases where, due to query complexity, data retrieval speeds are lower than they would be with a fast, simple query. Use parallel retrieval to retrieve large data sets up to the capacity limits defined for an API service.

The CDF APIs support parallel retrieval through the `partition` parameter, where the parameter is in the format `m/n` and `n` represents the number of partitions to split the entire data set into. CDF supports a maximum of 10 concurrent partitions. Attempts to retrieve a higher number of parallel partitions may be detrimental to client data retrieval performance due to throttling.

:::info note
Parallel retrieval doesn't act as a speed multiplier on optimally running queries. If adding more partitions doesn't improve data retrieval performance, don't add them.
:::

### Example

This example downloads an entire data set in parallel by splitting it into 10 partitions and doing the following requests with `m` running from 1 to 10:

```http
HTTP GET /events?type=WORKORDER&partition=1/10
```

```http
HTTP GET /events?type=WORKORDER&partition=2/10
```

```http
HTTP GET /events?type=WORKORDER&partition=3/10
```

Each of the results looks similar to this:

```json
{
  "items": [
    {
      "startTime": 1266130800000,
      "endTime": 1284328234000,
      "type": "Workorder",
      "subtype": "VAL",
      "assetIds": [4650652196144007],
      "source": "cdf-tenant",
      "id": 206950949774808,
      "lastUpdatedTime": 1539151642457,
      "createdTime": 1533747616899
    }
  ],
  "nextCursor": "bU-qc7X1jKMUbXgOYdONu2kRvpsoc60v9qh0Votm4MT0LaH0J0VVhVLoorSh8j_j"
}
```

Follow the `nextCursor` in each response until each partition is drained.

:::info note
Make sure that you send the filter parameters to each request that follows.
:::

<!--- How should we spell data point: datapoint or data point? --->

## Datapoint pagination

Datapoints, including aggregates, can use pagination similar to other endpoints. The first datapoint on the second page will follow immediately after the last datapoint on the first page.

The main difference to other endpoints is that you will receive a `nextCursor` for each time series that has more data and that the `nextCursor` fields are *inside* the elements of the `items` array. In further requests, you must combine each cursor alongside its corresponding time series.

If the other parameters are equal, you may combine `(time series, cursor)` pairs from different requests.

You may also drop the time series you've completed and increase the limit on the remaining time series. Requests with smaller time series with a higher limit will be more effective than paging.

**Datapoint cursors** are _opaque_ and not designed to be meaningful to users. The cursors aren't encrypted but shouldn't be reverse-engineered to construct a new cursor to supply to the API.

Cursors are valid for a reasonable time after the original API call, also if they've been used in a follow-up request. Still, we don't guarantee a minimum validity time, and you shouldn't store cursors to use them after a protracted time.

Cursors from different requests don't affect each other, and you can concurrently submit distinct series of cursor-based requests even if they cover the same period.

In general, distinct API calls aren't atomic with each other, also not when using cursors. The cursor doesn't "remember" the state of the data points at the time of the original request.

An API call that provides a cursor that was returned by a previous call will only see points with a timestamp after the last timestamp in the previous call. However, the second call **might see** some data points that didn't exist at the time of the first call and **might not see** some data points that did exist at the time of the first call. Also, the second call may let you see a partial result from an unrelated, concurrent request. If the unrelated request adds or deletes data points in a range that straddles the timestamp that the cursor points to, the second cursor-based request might see the result of the other request even though the first request that produced the cursor didn't.

### Example

```json
{
  "items": [
    {
      "id": 154872395793457,
      "externalId": "thermometer",
      "isString": false,
      "isStep": false,
      "unit": "kelvin",
      "datapoints": [
        {
          "timestamp": 427629600000,
          "value": 183.95
        }
      ],
      "nextCursor": "jkewklavngoatOPari4nsLGNKd453sa-asl"
    },
    {
      "id": 1897391123,
      "externalId": "CO2",
      "isString": false,
      "isStep": false,
      "unit": "ppm",
      "datapoints": [
        {
          "timestamp": 1654027200000,
          "average": 420.99
        }
      ],
      "nextCursor": "awnl2ao42aiNawKa24Lk4vab021lnkaj"
    }
  ]
}
```

Now, take a look at the example request. Each time series includes the corresponding cursor.

```http
HTTP POST /timeseries/data/list
```

```json
{
  "items": [
    {
      "externalId": "thermometerA",
      "cursor": "jkewklavngoatOPari4nsLGNKd453sa-asl"
    },
    {
      "externalId": "CO2",
      "cursor": "awnl2ao42aiNawKa24Lk4vab021lnkaj",
      "aggregates": ["average"],
      "granularity": "1d"
    }
  ],
  "start": 400000000000,
  "end": 1700000000000,
  "limit": 1
}
```

<details>
<summary>Click here to view the full example.</summary>

All requests are sent to this endpoint:

```http
HTTP POST /timeseries/data/list
```

#### Regular query

Query for common datapoints without cursors:

```json
{
  "items": [
    {
      "externalId": "thermometerA"
    },
    {
      "externalId": "thermometerB"
    }
  ],
  "start": 400000000000,
  "end": 1700000000000,
  "limit": 1
}
```

Response from the first query:

```json
{
  "items": [
    {
      "id": 154872395793457,
      "externalId": "thermometerA",
      "isString": false,
      "isStep": false,
      "unit": "kelvin",
      "datapoints": [
        {
          "timestamp": 427629600000,
          "value": 183.95
        }
      ],
      "nextCursor": "jkewklavngoatOPari4nsLGNKd453sa-asl"
    },
    {
      "id": 254872395793458,
      "externalId": "thermometerB",
      "isString": false,
      "isStep": false,
      "unit": "kelvin",
      "datapoints": [
        {
          "timestamp": 427629600000,
          "value": 183.95
        }
      ]
    }
  ]
}
```

- **thermometerA** may have more datapoints in the range. Use the cursor to retrieve them.
- **thermometerB** has reached the latest datapoint in the range. Increase `start`/`end` to retrieve more datapoints.

#### Aggregate query

```json
{
  "items": [
    {
      "externalId": "CO2-A",
      "aggregates": ["average"],
      "granularity": "1d"
    },
    {
      "externalId": "CO2-B",
      "aggregates": ["average"],
      "granularity": "1d"
    }
  ],
  "start": 400000000000,
  "end": 1700000000000,
  "limit": 1
}
```

Aggregate response:

```json
{
  "items": [
    {
      "id": 1897391123,
      "externalId": "CO2-A",
      "isString": false,
      "isStep": false,
      "unit": "ppm",
      "datapoints": [
        {
          "timestamp": 1654027200000,
          "average": 420.99
        }
      ],
      "nextCursor": "awnl2ao42aiNawKa24Lk4vab021lnkaj"
    },
    {
      "id": 2897391124,
      "externalId": "CO2-B",
      "isString": false,
      "isStep": false,
      "unit": "ppm",
      "datapoints": [
        {
          "timestamp": 1654027200000,
          "average": 420.99
        }
      ]
    }
  ]
}
```

- **CO2-A** have more aggregate datapoints in the range. Use the cursor to retrieve them.
- **CO2-B** has reached the latest interval in the range with at least one aggregate. Increase start/end to fetch more aggregates.

#### Request with cursors

Each **externalId** is paired with the corresponding cursor. CDF requests time series from the first and second requests because the parameters are compatible. In this case, the start and end are equal.

```http
HTTP POST /timeseries/data/list
```

```json
{
  "items": [
    {
      "externalId": "thermometerA",
      "cursor": "jkewklavngoatOPari4nsLGNKd453sa-asl"
    },
    {
      "externalId": "CO2-A",
      "cursor": "awnl2ao42aiNawKa24Lk4vab021lnkaj",
      "aggregates": ["average"],
      "granularity": "1d"
    }
  ],
  "start": 400000000000,
  "end": 1700000000000,
  "limit": 1
}
```

Response from request with cursors:

```json
{
  "items": [
    {
      "id": 154872395793457,
      "externalId": "thermometerA",
      "isString": false,
      "isStep": false,
      "unit": "kelvin",
      "datapoints": [
        {
          "timestamp": 1263769200000,
          "value": 249.15
        }
      ],
      "nextCursor": "NIaoitj4jwlkrafjalkdjfalgkjladkjflgawa"
    },
    {
      "id": 1897391123,
      "externalId": "CO2-A",
      "isString": false,
      "isStep": false,
      "unit": "ppm",
      "datapoints": [
        {
          "timestamp": 1656626400000,
          "average": 420.95
        }
      ]
    }
  ]
}
```

</details>
