---
pagination_next: null
pagination_prev: null
title: Data ingestion
---
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Ingest data with templates


:::caution Deprecation
Templates is being superseded with the released data modeling capabilities. See [Data modeling](https://docs.cognite.com/cdf/data_modeling/) for more information.
:::

For each type you can declare 3 types of `properties`: **Primitives**, **CDF resources**, and **Relationships**. Additionally, each of the properties can be a list. To learn more about data modelling, see [Model data with templates](./data-model-spec.md).

<!-- trigger -->

Ingesting or populating data for templates is a way to declare the value for each field of a new instance, as identified by an `externalId`. Declaring a value for a field means resolving a field for a **field resolver**. Each field of an instance is bound to a field resolver.

This section outlines the different ways to ingest data with field resolvers.

|               | Ingesting from / as   | UI  | SDK                                                                          |
| ------------- | --------------------- | --- | ---------------------------------------------------------------------------- |
| Primitives    | String                | ✅  | ✅ via [ConstantResolver](#resolve-to-a-primitive)                           |
|               | Long                  | ✅  | ✅ via [ConstantResolver](#resolve-to-a-primitive)                           |
|               | Int                   | ✅  | ✅ via [ConstantResolver](#resolve-to-a-primitive)                           |
|               | Float                 | ✅  | ✅ via [ConstantResolver](#resolve-to-a-primitive)                           |
|               | Boolean               | ✅  | ✅ via [ConstantResolver](#resolve-to-a-primitive)                           |
| CDF resources | Asset                 | ✅  | ✅ via [ConstantResolver](#resolve-to-a-cdf-resource)                        |
|               | Time series           | ✅  | ✅ via [ConstantResolver](#resolve-to-a-cdf-resource)                        |
|               | Synthetic time series | ❌  | ✅ via [SyntheticTimeSeriesResolver](#synthetictimeseriesresolver)           |
|               | Sequence              | ❌  | ✅ via [ConstantResolver](#resolve-to-a-cdf-resource)                        |
|               | File                  | ❌  | ✅ via [ConstantResolver](#resolve-to-a-cdf-resource)                        |
|               | Event                 | ❌  | ❌                                                                           |
|               | RAW                   | ❌  | ✅ via [RawResolver](#rawresolver)                                           |
| Lists         |                       | ❌  | ✅ via [ConstantResolver](#resolve-to-lists-of-primitives-and-relationships) |
| Relationships |                       | ❌  | ✅ via [ConstantResolver](#resolve-to-relationships)                         |

## ConstantResolver

Use the ConstantResolver to resolve a field to a value that's only persisted in the template group itself.

### Resolve to a primitive

```graphql
type MyType @template {
  myStringField: String
  myIntField: Int
  myFloatField: Float
  myBooleanField: Boolean
  child: NonTemplateType
}

type NonTemplateType { # Non template type
  textField: String
}
```

<Tabs>
  <TabItem value="py" label="Python" default>



```python
TemplateInstance(
    external_id="id-for-instance",
    template_name="MyType",
    field_resolvers={
        "myStringField": ConstantResolver(value="Foo") # String
        "myIntField": ConstantResolver(value=1) # Long, Int
        "myFloatField": ConstantResolver(value=1.1) # Float
        "myBooleanField": ConstantResolver(value=True) # Boolean
        "child": ConstantResolver(value={"textField": "hello world"}) # Object
    },
)
```

  </TabItem>
  <TabItem value="js" label="JavaScript" default>

```typescript
const instance: ExternalTemplateInstance = {
  externalId: 'id-for-instance',
  templateName: 'MyType',
  fieldResolvers: {
    myStringField: new ConstantResolver('Foo'), // String
    myIntField: new ConstantResolver(1), // Long, Int
    myFloatField: new ConstantResolver(1.1), // Float
    myBooleanField: new ConstantResolver(true), // Boolean
    child: new ConstantResolver({ textField: 'hello world' }), // Object
  },
};
```

  </TabItem>
</Tabs>

### Resolve to a CDF resource

```graphql
type MyType @template {
  myAssetField: Asset
  myTimeSeriesField: TimeSeries
  myFileField: File
  mySequenceField: Sequence
}
```

<Tabs>
  <TabItem value="py" label="Python" default>



```python
instance = TemplateInstance(
    external_id="id-for-instance",
    template_name="MyType",
    field_resolvers={
        "myAssetField": ConstantResolver(value="asset-external-id") # Asset's External ID
        "myTimeSeriesField": ConstantResolver(value="ts-external-id") # Time Series' External ID
        "myFileField": ConstantResolver(value="file-external-id") # File's External ID
        "mySequenceField": ConstantResolver(value="sequence-external-id") # Sequence's External ID
    },
)
```

  </TabItem>
  <TabItem value="js" label="JavaScript" default>



```typescript
const instance: ExternalTemplateInstance = {
  externalId: 'id-for-instance',
  templateName: 'MyType',
  fieldResolvers: {
    myAssetField: new ConstantResolver('asset-external-id'), // Asset's External ID
    myTimeSeriesField: new ConstantResolver('ts-external-id'), // Time Series' External ID
    myFileField: new ConstantResolver('file-external-id'), // File's External ID
    mySequenceField: new ConstantResolver('sequence-external-id'), // Sequence's External ID
  },
};
```

  </TabItem>
</Tabs>

### Resolve to relationships

Built-in types and templates can relate to each other.
You can define the relationship between templates by specifying the `externalId` of the relating instances.

```graphql
type Well @template {
  name: String
}
type System @template {
  sub_well: Well # References the type above
}
```

<Tabs>
  <TabItem value="py" label="Python" default>



```python
# Well instance
well_instance = TemplateInstance(
    external_id="myWell",
    template_name="Well",
    field_resolvers={
        "name": ConstantResolver(value="Favorite Well") # String field for Well's name
    },
)
# System instance referencing Well
system_instance = TemplateInstance(
    external_id="mySystem",
    template_name="MyType",
    field_resolvers={
        "sub_well": ConstantResolver(value="myWell") # Referencing the well via external_id
    },
)
```

  </TabItem>
  <TabItem value="js" label="JavaScript" default>



```typescript
// Well instance
const wellInstance: ExternalTemplateInstance = {
  externalId: 'myWell',
  templateName: 'Well',
  fieldResolvers: {
    name: new ConstantResolver('Favorite Well'), // String field for Well's name
  },
};
// System instance referencing Well
const systemInstance: ExternalTemplateInstance = {
  externalId: 'mySystem',
  templateName: 'MyType',
  fieldResolvers: {
    sub_well: new ConstantResolver('myWell'), // Referencing the well via external_id
  },
};
```

  </TabItem>
</Tabs>

### Resolve to lists of primitives and relationships

Whether it's a one-to-one or a one-to-many relationship, the `externalId`s can originate from any field resolver that resolves to a string or an array of strings.

```graphql
type Well @template {
  name: String
}
type MyType @template {
  tags: [String]
  myAssetFields: [Asset]
  wells: [Well]
}
```

<Tabs>
  <TabItem value="py" label="Python" default>



```python
# Well instance
well_instance = TemplateInstance(
    external_id="myWell",
    template_name="Well",
    field_resolvers={
        "name": ConstantResolver(value="Favorite Well") # String field for Well's name
    },
)
instance = TemplateInstance(
    external_id="id-for-instance",
    template_name="MyType",
    field_resolvers={
        "tags": ConstantResolver(value=["foo", "bar"]) # Strings in an array
        "myAssetFields": ConstantResolver(value=["asset-external-id"]) # Asset's External IDs, in an array
        "wells": ConstantResolver(value=["myWell"]) # Wells' externalIds, in an array
    },
)
```

  </TabItem>
  <TabItem value="js" label="JavaScript" default>



```typescript
// Well instance
const wellInstance: ExternalTemplateInstance = {
  externalId: 'myWell',
  templateName: 'Well',
  fieldResolvers: {
    name: new ConstantResolver('Favorite Well'), // String field for Well's name
  },
};
const instance: ExternalTemplateInstance = {
  externalId: 'id-for-instance',
  templateName: 'MyType',
  fieldResolvers: {
    tags: new ConstantResolver(['foo', 'bar']), // Strings in an array
    myAssetFields: new ConstantResolver(['asset-external-id']), // Asset's External IDs, in an array
    wells: new ConstantResolver(['myWell']), // Wells' externalIds, in an array
  },
};
```

  </TabItem>
</Tabs>

**Learn more:**

- [Python documentation for ConstantResolver](https://cognite-sdk-python.readthedocs-hosted.com/en/latest/deprecated.html)
- [JavaScript documentation for ConstantResolver](https://cognite-sdk-python.readthedocs-hosted.com/en/latest/deprecated.html)

## SyntheticTimeSeriesResolver

Use the SyntheticTimeSeriesResolver to resolve a field of type `TimeSeries` to a synthetic time series in CDF. The only required argument is the synthetic time series expression, but you can also set the description, the unit, and if it's a step series or a string series.

```graphql
type MyType @template {
  myTimeSeriesField: SyntheticTimeSeries
}
```

<Tabs>
  <TabItem value="py" label="Python" default>



```python
instance = TemplateInstance(
    external_id="id-for-instance",
    template_name="MyType",
    field_resolvers={
        "myTimeSeriesField": SyntheticTimeSeriesResolver(
            expression="sin(pow(TS{externalId='Norway_confirmed'}, 2))",
            description="Weird sin time series",
            is_step=False,
            is_string=False,
            unit="radians",
        ) # Synthetic time series definition
    },
)
```

  </TabItem>
  <TabItem value="js" label="JavaScript" default>



```typescript
const instance: ExternalTemplateInstance = {
  externalId: 'id-for-instance',
  templateName: 'MyType',
  fieldResolvers: {
    myTimeSeriesField: new SyntheticTimeSeriesResolver(
      "sin(pow(TS{externalId='Norway_confirmed'}, 2))", // expression
      'some name', // name
      {}, // metadata
      'Weird sin time series', // description
      false, // isStep
      false, // isString
      'radians' // unit
    ), // Synthetic time series definition
  },
};
```

  </TabItem>
</Tabs>

**Learn more:**

- [Python documentation for SyntheticTimeSeriesResolver](https://cognite-sdk-python.readthedocs-hosted.com/en/latest/deprecated.html)
- [JavaScript documentation for SyntheticTimeSeriesResolver](https://cognitedata.github.io/cognite-sdk-js/beta/classes/_stable_src_types_.synthetictimeseriesresolver.html)

## RawResolver

Use the RawResolver to resolve a field to a value from a specific row and column in RAW.

There is no data model field that's of the type RAW. It simply resolves to the table -> row -> column's value as a value.

```graphql
type MyType @template {
  someValue: String
}
```

<Tabs>
  <TabItem value="py" label="Python" default>



```python
instance = TemplateInstance(
    external_id="id-for-instance",
    template_name="MyType",
    field_resolvers={
        "someValue": RawResolver(
            db_name="SomeDb",
            table_name="SomeTable",
            row_key="someRow",
            column_name="someColumn"
        ) # RAW resolver definition
    },
)
```

  </TabItem>
  <TabItem value="js" label="JavaScript" default>



```typescript
const instance: ExternalTemplateInstance = {
  externalId: 'id-for-instance',
  templateName: 'MyType',
  fieldResolvers: {
    someValue: new RawResolver(
      'SomeDb', // db name
      'SomeTable', // table name
      'someRow', // row key
      'someColumn' // column name
    ), // RAW resolver definition
  },
};
```

  </TabItem>
</Tabs>

**Learn more:**

- [Python documentation for RawResolver](https://cognite-sdk-python.readthedocs-hosted.com/en/latest/deprecated.html)
- [Javascript Documentation for RawResolver](https://cognitedata.github.io/cognite-sdk-js/beta/classes/_stable_src_types_.rawresolver.html)

## Complete example

```graphql
type System @template {
  name: String!
  tags: [String]!
  wells: [Well]
  parentSystem: System
}
type Well @template {
  pressureSensor: SyntheticTimeSeries
  assets: [Asset]
  sourceName: String
  weight: Float
}
```

<Tabs>
  <TabItem value="py" label="Python" default>



```python
TemplateInstance(
    external_id="system1",
    template_name="System",
    field_resolvers={
        "name": RawResolver(
            db_name="SomeDb",
            table_name="SomeTable",
            row_key="someRow",
            column_name="someColumn"
        ) # RAW resolver definition
        "tags": ConstantResolver(value=["foo", "bar"]),
        "wells": ConstantResolver(value=["well1"]),
    },
)
TemplateInstance(
    external_id="system2",
    template_name="System",
    field_resolvers={
        "name": ConstantResolver(value="name"),
        "tags": ConstantResolver(value=["foo", "bar", "star"]),
        "wells": ConstantResolver(value=["well2"]),
        "parentSystem": ConstantResolver(value="system1"),
    },
)
TemplateInstance(
    external_id="well1",
    template_name="Well",
    field_resolvers={
        "pressureSensor": SyntheticTimeSeriesResolver(
            expression="sin(pow(TS{externalId='Norway_confirmed'}, 2))",
            description="Weird sin time series",
            is_step=False,
            is_string=False,
            unit="radians",
        ) # Synthetic time series definition,
        "assets": ConstantResolver(value=["someAssetExtId", "someOtherAssetExtId"]),
        "sourceName": ConstantResolver(value="someString"),
        "weight": ConstantResolver(value=12.3),
    },
)
TemplateInstance(
    external_id="well2",
    template_name="Well",
    field_resolvers={
        "assets": ConstantResolver(value=["someAssetExtId", "someOtherAssetExtId"]),
        "sourceName": ConstantResolver(value="someString"),
        "weight": ConstantResolver(value=12.3),
    },
)
```

  </TabItem>
  <TabItem value="js" label="JavaScript" default>



```typescript
const system1: ExternalTemplateInstance = {
  externalId: 'system1',
  templateName: 'System',
  fieldResolvers: {
    name: new RawResolver('SomeDb', 'SomeTable', 'someRow', 'someColumn'), // RAW resolver definition
    tags: new ConstantResolver(['foo', 'bar']),
    wells: new ConstantResolver(['well1']),
  },
};
const system2: ExternalTemplateInstance = {
  externalId: 'system2',
  templateName: 'System',
  fieldResolvers: {
    name: new ConstantResolver('name'),
    tags: new ConstantResolver(['foo', 'bar', 'star']),
    wells: new ConstantResolver(['well2']),
    parentSystem: new ConstantResolver('system1'),
  },
};
const well1: ExternalTemplateInstance = {
  externalId: 'well1',
  templateName: 'Well',
  fieldResolvers: {
    pressureSensor: new SyntheticTimeSeriesResolver(
      "sin(pow(TS{externalId:'Norway_confirmed'}, 2))",
      'name',
      {},
      'Weird sin time series',
      false,
      false,
      'radians'
    ), // Synthetic time series definition,
    assets: new ConstantResolver(['someAssetExtId', 'someOtherAssetExtId']),
    sourceName: new ConstantResolver('someString'),
    weight: new ConstantResolver(12.3),
  },
};
const well2: ExternalTemplateInstance = {
  externalId: 'well2',
  templateName: 'Well',
  fieldResolvers: {
    assets: new ConstantResolver(['someAssetExtId', 'someOtherAssetExtId']),
    sourceName: new ConstantResolver('someString'),
    weight: new ConstantResolver(12.3),
  },
};
```

  </TabItem>
</Tabs>

## SDK documentation

- [Cognite Python SDK](https://cognite-sdk-python.readthedocs-hosted.com/en/latest/deprecated.html)
- [Cognite JavaScript SDK (Beta)](https://cognitedata.github.io/cognite-sdk-js/beta/classes/_stable_src_api_templates_templateinstancesapi_.templateinstancesapi.html)
