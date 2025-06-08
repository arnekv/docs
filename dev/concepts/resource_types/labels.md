---
pagination_next: null
pagination_prev: null
---

# Labels

<!--What are Labels?  Generic overview information-->

With **labels** you can create a predefined set of **managed terms** that you can use to annotate and group **assets**, **files**, and **relationships**.

You can organize the labels in a way that makes sense in your business and use the labels to make it easier to find what you want. For example, you can create a label called _pump_ and apply it to all asset resources that represent pumps, and then filter assets to see only pumps.

You can also use labels to define and manage the available **types** for [relationship](relationships.md) resources. For example, you can define labels like these and use them as relationship types:

- `flowsTo` - to describe the flow between assets.
- `belongsTo` - to describe that a file resource belongs to a particular asset resource.
- `isParentOf` - to build a hierarchy of assets.
- `implements` - to describe that a physical item implements a functional asset at a specific point in time.

## About labels

<!--What can I do with Labels?  -->

Each asset/file can have up to ten labels connected to it. You can filter assets/files by their labels to find a group of assets/files.

Typical use cases for labels include:

- Categorize assets into equipment types.
- Locate assets by equipment type.
- Define [relationship](relationships.md) types.
- Assign a document type label to files.

:::info TIP
See the [labels API documentation](/api#tag/Labels) for more information about how to work with labels.
:::

<!--Add examples to support and illustrate the concepts above -->

## Create a label

To create a label, you need to give it an ID and a name. The label ID must be an [`externalId`](../external_id.md).

This example request creates a label:

```http
POST /api/v1/projects/publicdata/labels
Content-Type: application/json

{
  "items": [
    {
      "externalId": "rotating-equipment",
      "name": "Rotating equipment",
      "description": "Pumps, compressors, turbines"
    },
    {
      "externalId": "PUMP",
      "name": "Pump",
      "description": "Pumps"
    }
  ]
}
```

## List labels

To list existing labels, use:

```http
POST /api/v1/projects/publicdata/labels/list
Content-Type: application/json

{
  "filter": {
    "name": "string",
    "externalIdPrefix": "string"
  },
  "limit": 10,
  "cursor": "ABC"
}
```

## Attach a label when you create a resource

To attach a label when you create a resource, you reference the labels through the [`externalId`](../external_id.md) of the label.

For example, to create a new asset resource and attach the labels `pump` and `rotating-equipment`, use this request:

```http
POST /api/v1/projects/publicdata/assets
Content-Type: application/json

{
  "items": [
    {
      "externalId": "MY_PUMP_ASSET",
      "labels": [
        { "externalId": "PUMP" },
        { "externalId": "rotating-equipment" }
      ]
    }
  ]
}
```

:::info note
You can attach a maximum of 10 labels to each resource.
:::

## Attach or remove labels to an existing resource

To attach (or remove) labels to an existing resource, you reference the labels through the [`externalId`](../external_id.md) of the label when you update the resource.

For example, to attach the label `pump` and remove label `rotating-equipment` to an existing asset, use the request:

```http
POST /api/v1/projects/publicdata/assets/update
Content-Type: application/json

{
  "items": [
    {
      "update": {
        "labels": {
          "add": [ { "externalId": "PUMP" } ],
          "remove": [ { "externalId": "rotating-equipment" } ]
        }
      },
      "externalId": "MY_PUMP_ASSET"
    }
  ]
}
```

:::info TIP
The API ignores the request if you try to attach a label that's already attached to the resource.
:::

:::info note
You can attach a maximum of 10 labels to each resource.
:::

## Remove labels from a resource

To remove labels from a resource, you reference the labels through the [`externalId`](../external_id.md) of the label when you update the resource.

For example, to remove the label `rotating-equipment` from an asset, use the request:

```http
POST /api/v1/projects/publicdata/assets/update
Content-Type: application/json

{
  "items": [
    {
      "externalId": "MY_PUMP_ASSET",
      "update": {
        "labels": {
          "remove": [
            { "externalId": "rotating-equipment" }
          ]
        }
      }
    }
  ]
}
```

:::info TIP
The API ignores the request if you try to remove a label that's not attached to the resource.
:::

## Filter resources by labels

To filter assets by a label, you reference the label through the [`externalId`](../external_id.md) of the label when you filter the assets.

For example, to retrieve only assets that have the label `rotating-equipment`, use the request:

```http
POST /api/v1/projects/publicdata/assets/list
Content-Type: application/json

 {
  "filter": {
    "labels": {
      "containsAny": [{ "externalId": "rotating-equipment" }]
    }
  }
}
```

To retrieve only assets that have the label `rotating-equipment` **or** `pump`:

```http
POST /api/v1/projects/publicdata/assets/list
Content-Type: application/json

  "filter": {
    "labels": {
      "containsAny" : [
        { "externalId": "rotating-equipment" },
        { "externalId": "PUMP" }
      ]
    }
  }
}
```

To retrieve only assets that have the labels `rotating-equipment` **and** `pump`:

```http
POST /api/v1/projects/publicdata/assets/list
Content-Type: application/json

  "filter": {
    "labels": {
      "containsAll" : [
        { "externalId": "rotating-equipment" },
        { "externalId": "PUMP" }
      ]
    }
  }
}
```

## Delete labels

To delete a label, you need to specify the [`externalId`](../external_id.md) of the label to delete.

This example request deletes a `rotating-equipment` label:

```http
POST /api/v1/projects/publicdata/labels/delete
Content-Type: application/json
{
  "items": [
    {
      "externalId": "rotating-equipment"
    }
  ]
}
```
