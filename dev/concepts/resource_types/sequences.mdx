---
pagination_next: null
pagination_prev: null
---

# Sequences

<!--What are sequences?  Generic overview information-->

In Cognite Data Fusion (CDF), a sequence is a generic **resource type** for indexing a series of **rows** by **row number**. Each **row** has one or more **columns** with either string or numeric data. Performance curves and various types of logs are examples of sequences.

## About sequences

<!--What can I do with sequences?  -->

An asset can have one or more sequences connected to it. You can analyze and visualize sequences to **draw inferences** from the data, for example, to visualize the expected performance of a generator given its load. Another common way to use sequences is to store large amounts of data index of which isn't linked to time.

<!--What is a row? More details + specific information for the Cognite implementation-->

A **row** is a piece of information associated with a **row number**. Each **row** has a number of **columns**, each containing an **integer**, **floating point**, or **string** value. When you create the sequence, you define the column types, and each sequence can have up to 200 columns. The rows are identified by their **row number**, a searchable positive 64-bit integer. You can't search by other columns than the row number. The rows don't have to be consecutive or start at a specific number.

Typical use cases for sequences include:

- Storing and retrieving an entire curve with an x-y dependency. In this case, use two floating-point columns so that the integer type on the row number doesn't restrict you..
- Storing a log based on distance or depth, with information in columns of different types for each entry. If you store the distance or depth as the row number (an integer), you may lose precision but can search for all events that occurred at a particular distance or depth.

:::info TIP
See the [sequences API documentation](/api#tag/Sequences) for more information about how to work with sequences.
:::

<!--Add examples to support and illustrate the concepts above -->

## Create a sequence

To create a sequence, you need to define the columns. It's optional, but highly recommended, to give the sequence itself an [`externalId`](../external_id.md). For columns, specify an `externalId` and a `valueType`.

This example request creates a sequence:

```http
POST /api/v1/projects/publicdata/sequences
Content-Type: application/json

{
    "items": [
        {
            "externalId": "XV_123/performance_curve",
            "name": "performance curve XV_123",
            "description": "Performance curve for the so-and-so compressor",
            "metadata":
                {
                    "source": "unliberated data store 42"
                },
            "assetId": 4324823473421,
            "columns": [
                {
                    "externalId": "load",
                    "valueType": "DOUBLE",
                    "metadata": {"unit": "kPa"}
                },
                {
                    "externalId": "performance",
                    "valueType": "LONG",
                    "description": "expected performance as % of maximum",
                    "metadata": {
                        "unit": "%",
                        "some other field": "some value"
                    }
                },
                {
                    "externalId": "quality",
                    "valueType": "STRING",
                    "name": "optional human readable name"
                }
            ]
        }
    ]
}
```

## Add rows to a sequence

To add rows to a sequence, you need to specify the sequence `id` or `externalId` along with a list of the columns you are inserting data into.

For example, to insert two rows in the sequence created in the previous example, use:

```http
POST /api/v1/projects/publicdata/sequences/data
Content-Type: application/json

{
    "items": [
        {
            "externalId": "XV_123/performance_curve",
            "columns": ["load","performance","quality"],
            "rows": [
                    {"rowNumber":1, "values":[123.45, 80, "POOR"] }
                    {"rowNumber":2, "values":[20.1, 95, "GOOD"] }
            ]
        }
    ]
}
```

Nulls can represent missing data.

## Retrieve rows from a sequence

To get the rows from a sequence, you can use the [`externalId`](../external_id.md) or the `id` of the sequence.
Optionally, specify the columns you want to retrieve. The default is to return all columns.

For example, to get the first 5 rows from a sequence by using the `externalId`, in this case `performance_curve`, use the request:

```http
POST /api/v1/projects/publicdata/sequences/data/list
Content-Type: application/json

{
    "limit": 5,
    "externalId": "example",
    "start": 0,
    "end": 5
}
```

:::info NOTE

- `end` is exclusive. Even if you specify a higher limit, the request returns a maximum of five rows.

- Unlike the endpoints for creating sequences and inserting rows, you can only request rows from a single sequence here.

- We skip rows where all column values are null/missing.

:::

The response will look similar to this:

```json
{
  "externalId": "example",
  "id": 7137123130454053,
  "columns": [
    {
      "externalId": "load",
      "valueType": "DOUBLE"
    },
    {
      "externalId": "performance",
      "valueType": "LONG"
    }
  ],
  "rows": [
    {
      "rowNumber": 1,
      "values": [0.0, 100]
    },
    {
      "rowNumber": 2,
      "values": [10.0, 95]
    },
    {
      "rowNumber": 3,
      "values": [20.0, 92]
    },
    {
      "rowNumber": 4,
      "values": [25.0, 85]
    },
    {
      "rowNumber": 5,
      "values": [30.0, 65]
    }
  ]
}
```

## Add (or modify) columns to a sequence

To add columns to a sequence, you need to use the [sequences update endpoint](/api#operation/updateSequences).
This endpoint can also remove or modify existing columns.

Data in removed columns are lost, whereas data in new columns default to null.

You can modify the name, externalId, description, or metadata fields. To modify and change the data type,
you need to delete and recreate the column. Modification won't affect the default ordering of columns.

For example, to update the metadata and externalId of a column, remove and add a column,
use the request:

```http
POST /api/v1/projects/publicdata/sequences/update
Content-Type: application/json

{
    "items": [
        {
            "externalId": "XV_123/performance_curve",
            "update": {
                "columns": {
                    "modify": [
                        {
                            "externalId": "load",
                            "update": {
                                "metadata": {
                                    "add": {
                                        "accuracy": "+/- 0.1 kPa"
                                    }
                                },
                                "externalId": {
                                    "set": "pressure"
                                }
                            }
                        }
                    ],
                    "remove": [
                        {
                            "externalId": "quality"
                        }
                    ],
                    "add": [
                        {
                            "externalId": "errors",
                            "description": "human readable error messages",
                            "valueType": "STRING"
                        }
                    ]
                }
            }
        }
    ]
}
```

## Paginate retrieved rows

When you get too many rows, you can use cursors to paginate through the results.

For example, to get a single specific column from all rows with row numbers between 0 and 999999, use the request:

```http
POST /api/v1/projects/publicdata/sequences/data/list
Content-Type: application/json

{
    "limit": 10000,
    "externalId": "big_example",
    "start": 0,
    "end": 1000000,
    "columns": ["performance"],
    "cursor": null
}
```

:::info NOTE
10000 is the maximum limit per request.
:::

The response will look similar to this:

```json
{
    "externalId": "big_example",
    "id": 21312313130454053,
    "columns": [
        {
            "externalId": "performance",
            "valueType":"LONG"
        }
    ],
    "rows": [
        {
            "rowNumber": 0,
            "values": [100]
        },
        {
            "rowNumber": 1,
            "values": [99]
        },
        <9997 rows ommitted>,
        {
            "rowNumber": 9999,
            "values": [42]
        }
    ],
    "nextCursor": "3zZmOn50qL9Kkhjwmrz602sHfQifGypzdqYEtQG3ajuU"
}
```

To retrieve the next 10000 rows, pass the same request with the `cursor` field changed to the value returned in `nextCursor`.
