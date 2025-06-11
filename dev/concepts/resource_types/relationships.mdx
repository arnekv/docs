---
pagination_next: null
pagination_prev: null
---

# Relationships

<!--What are Relationships?  Generic overview information-->

The **Relationships** resource type represents connections between resource objects in Cognite Data Fusion (CDF). Each relationship is between a source and a target object and is defined by a **relationship type** and the **external IDs** and **resource types** of the source and target objects. Optionally, a relationship can be time-constrained with a start and end time.

The `externalId` field uniquely identifies each relationship.

To define and manage the available relationship types, use the [labels](labels.md) resource type. For example, you can define and use the following labels as relationship types:

- `flowsTo` - to describe the flow between assets.
- `belongsTo` - to describe that a file resource belongs to a particular asset resource.
- `isParentOf` - to build a hierarchy of assets.
- `implements` - to describe that a physical item implements a functional asset at a specific point in time.

NOTE: These labels are informational only, and they're case-sensitive, for example, `flowsTo` and `FlowsTo` aren't the same.

## About relationships

<!--What can I do with relationships?  -->

Typically, asset resources originate from a maintenance system, and the hierarchical structure of the maintenance system often defines how the asset resources are organized in Cognite Data Fusion (CDF).

The **relationships** resource type lets you organize the assets in other structures **in addition to** the standard hierarchical asset structure.

For example, you can organize the assets by their physical **location**, where the grouping nodes in the hierarchy are buildings and floors rather than systems and functions. Another example is to build a graph structure that lets you navigate assets by mimicking their physical **connections** through wires or pipes. See the [Modeling asset structures with relationships](https://docs.cognite.com/cdf/concepts/model_asset_structures) for examples.

Before you create a relationship, make sure that the relationship type exists as a [label](labels.md). When you create a relationship, you must specify its type using the `externalId` of the relevant label.

When you create a relationship, it's good practice to add them to a data set for grouping and governance.

It's not a requirement that the source or target resource exist when you create a relationship. This lets you create relationships between objects that don't yet exist in CDF.

:::info TIP
See the [relationships API documentation](/api#tag/Relationships) for more information about how to work with relationships.
:::

<!--Add examples to support and illustrate the concepts above -->

## Create a relationship between two assets

To create a relationship, you need to give it an ID and a type and specify the source and target resource objects the relationship connects. The relationship ID must be an [`externalId`](/dev/concepts/external_id.md). The relationship type must be the `externalId` of a [label](./labels.md).

Create relationships within a data set for logical grouping and governance.

This example request creates a relationship:

```http
POST /api/v1/projects/publicdata/relationships
Host: api.cognitedata.com
Authorization: Bearer <token>
Content-Type: application/json

{
  "items": [
    {
      "externalId": "relationship_1",
      "dataSetId": 5514071318856557,
      "sourceExternalId": "asset_1",
      "sourceType": "asset",
      "targetExternalId": "asset_2",
      "targetType": "asset",
      "labels": [
        {
          "externalId": "flowsTo"
        }
      ]
    }
  ]
}
```

## Create a relationship between a file and an asset

You can use relationships to create links between any resources by specifying the `sourceType` and `targetType`.

This example shows how to create a relationship of type `belongsTo` between a file and an asset resource.

```http
POST /api/v1/projects/publicdata/relationships
Host: api.cognitedata.com
Authorization: Bearer <token>
Content-Type: application/json

{
  "items": [
    {
      "externalId": "relationship_2",
      "dataSetId": 5514071318856557,
      "sourceExternalId": "file_1",
      "sourceType": "file",
      "targetExternalId": "asset_1",
      "targetType": "asset",
      "labels": [
        {
          "externalId": "belongsTo"
        }
      ]
    }
  ]
}
```

## Create a time-ranged relationship between two assets

When you have physical equipment as part of the asset resources, you can use relationships to capture how physical equipment serves at different functional locations over time. You can specify the timespan a relationship is valid for by specifying the `startTime` and `endTime` properties.

This example shows how to create a relationship between two assets with a time range.

```http
POST /api/v1/projects/publicdata/relationships
Host: api.cognitedata.com
Authorization: Bearer <token>
Content-Type: application/json

{
  "items": [
    {
      "externalId": "relationship_3",
      "dataSetId": 5514071318856557,
      "sourceExternalId": "asset_1",
      "sourceType": "asset",
      "targetExternalId": "asset_2",
      "targetType": "asset",
      "startTime": 1514768406,
      "endTime": 1577840406,
      "labels": [
        {
          "externalId": "flowsTo"
        }
      ]
    }
  ]
}
```

## List relationships

To list existing relationships, use:

```http
POST /api/v1/projects/publicdata/relationships/list
Host: api.cognitedata.com
Authorization: Bearer <token>
Content-Type: application/json

{}
```

## List relationships for a particular asset

Relationships are directional. Therefore, we split this into two calls, one where the asset is the source and one where the asset is the target.

```http
POST /api/v1/projects/publicdata/relationships/list
Host: api.cognitedata.com
Authorization: Bearer <token>
Content-Type: application/json

{
  "filter": {
    "sourceTypes": ["asset"],
    "sourceExternalIds": ["asset_1"]
  }
}
```

```http
POST /api/v1/projects/publicdata/relationships/list
Host: api.cognitedata.com
Authorization: Bearer <token>
Content-Type: application/json

{
  "filter": {
    "targetTypes": ["asset"],
    "targetExternalIds": ["asset_1"]
  }
}
```

## List relationships of a particular type for a particular asset

To retrieve all `flowsTo` relationships of `asset_1`.

```http
POST /api/v1/projects/publicdata/relationships/list
Host: api.cognitedata.com
Authorization: Bearer <token>
Content-Type: application/json

{
  "filter": {
    "sourceTypes": ["asset"],
    "sourceExternalIds": ["asset_1"],
    "labels": {"containsAll": [{"externalId": "flowsTo"}]}
  }
}
```

## List relationship for an asset at a specific point in time

To list all time-ranged relationships of an asset with type `implements` and valid at a specific point in time, use:

```http
POST /api/v1/projects/publicdata/relationships/list
Host: api.cognitedata.com
Authorization: Bearer <token>
Content-Type: application/json

{
  "filter": {
    "sourceTypes": ["asset"],
    "sourceExternalIds": ["asset_1"],
    "activeAtTime": {
      "min": 1601284751,
      "max": 1601284751
    },
    "labels": {"containsAll": [{"externalId": "implements"}]}
  }
}
```

<!-- ## Update a relationship -->

## Delete a relationship

To delete a relationship, use:

```http
POST /api/v1/projects/publicdata/relationships/delete
Host: api.cognitedata.com
Authorization: Bearer <token>
Content-Type: application/json

{
  "items": [{"externalId": "relationship_1"}]
}
```
