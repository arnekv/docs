---
pagination_next: null
pagination_prev: null
toc_max_heading_level: 4
---

# Filtering

Cognite Data Fusion (CDF) supports boolean logic using _AND_, _OR_, and _NOT_ to build complex queries. You can also combine resource item attribute filters, such as _equals_, _prefix_, and _range_.

The default set of CDF filters lets you filter all CDF resources in these contexts:

- **CDF resources**, for example, assets and events, by primary fields, metadata keys, and values.

- **Full-text search results**. Use filters to narrow down the returned search results. For example, you can use a filter to return only "PDFs from last week" from your full-text search query.

- **Aggregate results**. Use filters to reduce the number of returned aggregate buckets. For example, you can use a filter to return "metadata key names and their count for all keys with the prefix character '\_'.

## Serialization/deserialization

You can express filters as JSON. The root must always be an object.

## Properties

CDF uses the same grammar to express a reference everywhere the API uses a resource attribute: a JSON array of strings to express the attribute/property. This is true for _leaf filters_, _aggregate_, _sort_, _etc_.

There are no naming constraints for the allowed property names. CDF uses "property" for **leaf filters** to refer to the field(s) the filter operates on.

### Example: nesting and sorting

In the example object below, you can refer to the nested values as `["metadata", "../type"]`. Or, you can use another nested property, `["metadata", "type"]`, and then address the un-nested property as `["type"]`.

```json
{
  "id": 1234,
  "metadata": {
    "../type": "critical",
    "type": "high"
  },
  "type": "alert"
}
```

To sort the `/list` result on _metadata.type_ and _type_ properties, send the following request:

```json
{
  "sort": [
    {
      "property": ["metadata", "type"],
      "order": "asc",
      "nulls": "last"
    },
    {
      "property": ["type"],
      "order": "desc"
    }
  ]
}
```

:::note
You can sort by several properties in the same query.
:::

### Example: filtering

This example defines a filter using JSON:

```json JSON
{
  "or": [
    {
      "not": {
        "and": [
          {
            "equals": {
              "property": ["type"],
              "value": "alert"
            }
          },
          {
            "in": {
              "property": ["subtype"],
              "values": ["critical", "warning"]
            }
          },
          {
            "range": {
              "property": ["rating"],
              "gte": 5,
              "lt": 10
            }
          }
        ]
      }
    },
    {
      "and": [
        {
          "equals": {
            "property": ["metadata", "priority"],
            "value": "highest"
          }
        },
        {
          "equals": {
            "property": ["subtype"],
            "value": "critical"
          }
        }
      ]
    }
  ]
}
```

A simplified, less verbose version could look like this:

```
(
    ("metadata.priority"=highest AND subtype=critical)
    OR
    (
        NOT (type=alert AND subtype IN (critical, warning) AND 5 <= rating < 10)
    )
)
```

Even if the JSON object is more verbose, the object is easy to express and handle programmatically. JSON is also **unambiguous** and doesn't impose rules on naming attributes/properties. You also can't mistake a property name for a keyword in JSON.

Below is another example of using JSON to express a simple intent: "_filter items where the attribute `source` is equal to the value `git`_."

```json
{
  "equals": {
    "property": ["source"],
    "value": "git"
  }
}
```

## Filter language specification

| Filter      | Syntax                                                                                                    |
| ----------- | --------------------------------------------------------------------------------------------------------- |
| filter       | `<bool_filter> \| <leaf_filter>`                                                                            |
| bool_filter  | `<and> \| <or> \| <not>`                                                                                  |
| and         | `{"and": [<filter>, …]}`                                                                                   |
| or          | `{"or": [<filter>, …]}`                                                                                    |
| not         | `{"not": <filter>}`                                                                                        |
| property    | `<list_of_strings>`                                                                                       |
| leaf_filter  | `<equals> \| <in> \| <range> \| <prefix> \| <exists> \| <containsAny> \| <containsAll>`                    |
| equals      | `{"equals": {"property": <property>, "value": <string_value>}}`                                           |
| in          | `{"in": {"property": <property>, "values": <list_value>}}`                                                |
| range       | `{"range": {"property": <property>, "(gte\|gt\|lte\|lt)": <value1> [, "(gte\|gt\|lte\|lt)": <value\2>]}}` |
| prefix       | `{"prefix": {"property": <property>, "value": <string_value>}}`                                            |
| exists      | `{"exists": {"property": <property>}}`                                                                    |
| containsAny | `{"containsAny": {"property": <property>, "values": <list_value>}}`                                       |
| containsAll | `{"containsAll": {"property": <property>, "values": <list_value>}}`                                       |

:::info
Different CDF resources may support a different subset of the existing leaf filters. Refer to the documentation for the specific CDF resource for details.
:::

## Boolean filters

### AND

To express boolean **AND** logic, use the `and` filter expressed in JSON:

`{"and": [ filter1, filter2, …, filterN]}`

### OR

To express boolean **OR** logic, use the `or` filter (expressed in JSON):

`{"or": [ filter1, filter2, …, filterN]}`

### NOT

To express the negated logic of a boolean value, use the `not` filter (expressed in JSON):

`{"not": filter}`

## Leaf filters

:::note
The supported leaf filters may differ depending on the CDF resource.
:::

### Equals

Filter items where the _property_ (named value) equals the specified value. This filter works for properties with values that aren't arrays/lists.

Filter items where the _property_ (name) equals the given name and has a specific value. This filter works for properties with values that aren't arrays/lists.

```json
{
  "equals": {
    "property": ["rootId"],
    "value": 12432432423
  }
}
```

### In

Filter items where the _property_ value equals one of the values specified in the list. This filter expects the value stored by the property to be a single value (not a list).

```json
{
  "in": {
    "property": ["datasetId"],
    "values": [1, 2, 3]
  }
}
```

### Range

Filter items where the _property_ value is in the range specified by the filter boundaries: _gt_|_gte_,|_lt_|_lte_.

```json
{
  "range": {
    "property": ["count"],
    "gte": 1,
    "lte": 2
  }
}
```

To compare the property, you must specify _gt_, _lt_, _lte_, or _gte_, and at least one boundary value.

Range behavior depends on the type of the property you are comparing with. The filter language does not require that typing must exist. If you store all the values as strings, the range filter will behave as if comparing string values.

This filter works for properties not typed as arrays/lists.

:::note
Some CDF resources don't support open-range filters. To avoid errors, specify both upper and lower boundaries. See the API documentation for details.
:::

### Prefix

Filter items where the _property_ value starts with the specified string. This filter works for properties that are not of type array/list.

```json
{
  "prefix": {
    "property": ["metadata", "vendor"],
    "value": "si"
  }
}
```

### Exists

Filter items where the _property_ value is something other than `null` (empty).
This filter works for properties of any type.

```json
{
  "exists": {
    "property": ["externalId"]
  }
}
```

### ContainsAny

Filter items where the _property_ values equal one or more of the specified values from the filter.
This filter works for properties that are of type array/list.

```json
{
  "containsAny": {
    "property": ["labels"],
    "values": ["pump", "tube"]
  }
}
```

### ContainsAll

Filter items where the values from the _property_ match all specified values.
This filter works for properties that are of type array/list.

```json
{
  "containsAll": {
    "property": ["assetIds"],
    "values": [23234324, 2423423]
  }
}
```

## Advanced Query filters

Advanced queries provide more robust data discovery and retrieval tools using `Search`, `Filter`, and `Aggregation` methods. Advanced search and filter API supports complex queries that combine simple operations, such as `equals`, `prefix`, `exists`, etc., using boolean operators `and`, `or`, and `not`. These methods apply to standard fields as well as metadata.

In the following section, we will outline the concepts and applications of different types of query filters with their API specifications for each supported service.

Since the filter operations leverage the CDF search architecture, they are consistent, expensive, and slower than CRUD operations. Consult with a professional on resource allocation and consumption.

### /filter

Use `/filter` for human and analytical queries, not bulk read or system-to-system sync operations.

### /list

Since `/list` provides a subset of the functionality available in `/filter`, use the `/filter` endpoint rather than `/list`.

For more information, see [List](https://developer.cognite.com/api/#tag/Assets/operation/getAssets).

### /search

`/search` returns results sorted by relevance score. Relevance score is automatically calculated by the CDF search architecture using a combination of text matching and scoring algorithms to determine the relevance of search results. The text-matching algorithm checks if the search query terms appear in the item's fields, such as title, description, or content.

> _Note: Searches can return high numbers of results._

:::tip
The `/filter` operation supersedes the`/search` functionality since it provides a fuzzy search operation within itself. Use the `/filter` operation for these types of queries.
:::

For more information on how search works for Docs API, see [Search for documents](https://developer.cognite.com/api/#tag/Documents/operation/documentsSearch/).

### /aggregate

Advanced aggregation APIs provide more powerful tools for data discovery and enhance the creation of better user interfaces and experiences. The supported aggregation types include the following:

| **Aggregation Type**   |      **Description**     |
|----------|:-------------|
| count |  Returns the count of items in the data set (i.e., number of Assets, Events, Time Series, etc.) |
| count (with properties) |  Returns the number of items in the data set with associated properties. It is useful if items in the data set have no associated properties.  |
| cardinalityValues |  Returns the number of instances that a property occurs in the data set |
| cardinalityProperties |  Returns the number of unique properties per metadata key that occur in the data set |
| uniqueValues |  Returns an array of the unique instances of metadata values per metadata key and the associated count of their occurrence |
| uniqueProperties |  Returns the number of unique properties that occur in the data set |

For more information, see [Aggregations in CDF](https://developer.cognite.com/dev/concepts/aggregation/).
