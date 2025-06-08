---
pagination_next: null
pagination_prev: null
---

# Upgrade

## Upgrade JavaScript SDK 2.x to 3.x

How to upgrade from SDK version 2.x to 3.x.

### Renamed interfaces

Some TypeScript interfaces have been renamed to have consistency between different resource types.
The most used ones are:

- GetTimeSeriesMetadataDTO -> Timeseries
- PostTimeSeriesMetadataDTO -> ExternalTimeseries
- DatapointsGetAggregateDatapoint -> DatapointAggregates
- FilesMetadata -> FileInfo

You can find a comprehensive list in the [changelog](https://github.com/cognitedata/cognite-sdk-js/blob/master/packages/stable/CHANGELOG.md#300-2020-08-03).

### Events aggregate endpoints moved to a sub-API

- `client.events.aggregate(...)` -> `client.events.aggregate.count()`
- `client.events.uniqueValuesAggregate(...)` -> `client.events.aggregate.uniqueValues(...)`

### Timestamp-to-Date auto-conversion

The JavaScript SDK automatically converts response timestamp values to date objects. This caused a [bug](https://github.com/cognitedata/cognite-sdk-js/issues/333) in the Raw API. The fix can introduce a breaking change if you use the Raw API, and possibly in other places.

### Helper classes removed

AssetClass, TimeSeriesClass, TimeSeriesList, and AssetList contained some helper methods that have been removed.

Most of these helpers can be re-implemented using other SDK calls.

Some examples:

**Asset.delete()**

```js
public async delete(options: DeleteOptions = {}) {
  return this.client.assets.delete(
    [{ id: this.id }],
    options
  );
}
```

**Asset.subtree()**

```js
public async subtree() {
  const { items } = await client.assets.list({
    filter: {
      assetSubtreeIds: [{ id: this.id }],
    }
  });
}
```

**TimeSeriesList.getAllDatapoints(...)**

```js
public getAllDatapoints = async (options: DatapointsMultiQueryBase = {}) => {
  const tsIds = this.map(({ id }) => ({ id }));
  return this.client.datapoints.retrieve({
    items: tsIds,
    ...options,
  });
};
```

### Removed client.assets.retrieveSubtree(...)

Copies the functionality of `Asset.subtree()` (read above).

### Change filter passing to client.timeseries.list

The function previously took a `TimeseriesFilter` as input and now takes a `TimeseriesFilterQuery`, which
corresponds to the [API spec](/api#operation/listTimeSeries). All filter-related fields should now be in the `filter` object.

#### Example

```js
// Old way
const timeseries = await client.timeseries.list({
  assetIds: [1, 2],
  unit: 'liter',
  limit: 100,
});

//New way
const timeseries = await client.timeseries.list({
  filter: { assetIds: [1, 2], unit: 'liter' },
  limit: 100,
});
```

## Upgrade JavaScript SDK 1.x to 2.x

How to upgrade from SDK version 1.x to 2.x.

### Constructor

In 2.x.x, you need to create a new SDK instance before you can use the SDK:

```js
const client = new CogniteClient({ appId: 'YOUR APPLICATION NAME' });
```

Next, you need to authenticate your client by calling one of these methods on the client:

With API key:

```js
client.loginWithApiKey({ project, apiKey });
```

With OAuth:

```js
client.loginWithOAuth({
  project: 'publicdata',
});
```

For more info about OAuth-authentication, see [here](https://github.com/cognitedata/cognite-sdk-js/blob/master/guides/authentication.md).

### Changes

| 1.x.x                                                            | 2.x.x                                                                                              |
| ---------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| **AUTHENTICATION**                                               |
| `sdk.Login.authorize(...)`                                       | See [constructor-section](#constructor)                                                            |
| `sdk.Login.loginWithApiKey(...)`                                 | See [constructor-section](#constructor)                                                            |
| `sdk.Logout.logout()`                                            | Removed                                                                                            |
| `sdk.Logout.retrieveLogoutUrl(...)`                              | Removed                                                                                            |
| **ASSETS**                                                       |
| `sdk.Assets.create([...])`                                       | `client.assets.create([...])`                                                                      |
| `sdk.Assets.retrieve(123)`                                       | `client.assets.retrieve([{id: 123}])`                                                              |
| `sdk.Assets.retrieveMultiple([123, 456])`                        | `client.assets.retrieve([{id: 123}, {id: 456}])`                                                   |
| `sdk.Assets.update(id, changes)`                                 | `client.assets.update([{id, update: changes}])`                                                    |
| `sdk.Assets.updateMultiple([{id, ...changes}])`                  | `client.assets.update([{id, update: changes}])`                                                    |
| `sdk.Assets.overwriteMultiple(...)`                              | Removed                                                                                            |
| `sdk.Assets.delete([id])`                                        | `client.assets.delete([{id: 123}])`                                                                |
| `sdk.Assets.list({name: 'abc'})`                                 | `client.assets.list({filter: {name: 'abc'}}).autoPagingToArray({limit: 1000})`                     |
| `sdk.Assets.listDescendants(id, params)`                         | Not implemented yet. Follow issue [here](https://github.com/cognitedata/cognite-sdk-js/issues/129) |
| `sdk.Assets.search({name: 'abc'})`                               | `client.assets.search({search: {name: 'abc'})`                                                     |
| **TIME SERIES**                                                  |
| `sdk.TimeSeries.create([...])`                                   | `client.timeseries.create([...])`                                                                  |
| `sdk.TimeSeries.retrieve(123)`                                   | `client.timeseries.retrieve([{id: 123}])`                                                          |
| `sdk.TimeSeries.retrieveMultiple([123, 456])`                    | `client.timeseries.retrieve([{id: 123}, {id: 456}])`                                               |
| `sdk.TimeSeries.update(id, changes)`                             | `client.timeseries.update([{id, update: changes}])`                                                |
| `sdk.TimeSeries.updateMultiple([{id, ...changes}])`              | `client.timeseries.update([{id, update: changes}])`                                                |
| `sdk.TimeSeries.list({limit: 20})`                               | `client.timeseries.list().autoPagingToArray({limit: 20})`                                          |
| `sdk.TimeSeries.search({name: 'abc'})`                           | `client.timeseries.search({search: {name: 'abc'}})`                                                |
| **DATA POINTS**                                                  |
| `sdk.Datapoints.insert(id, datapoints)`                          | `client.datapoints.insert([{id, datapoints}])`                                                     |
| `sdk.Datapoints.insertByName(...)`                               | Removed                                                                                            |
| `sdk.Datapoints.insertMultiple([{name, datapoints}])`            | Not supported using name. Use IDs instead: `client.datapoints.insert([{id, datapoints}])`          |
| `sdk.Datapoints.retrieve(id, params)`                            | `client.datapoints.retrieve({items: [{id}], ...params})`                                           |
| `sdk.Datapoints.retrieveByName(...)`                             | Removed (use ID instead)                                                                           |
| `sdk.Datapoints.retrieveMultiple(...)`                           | Not directly supported. Use `client.datapoints.retrieve`                                           |
| `sdk.Datapoints.retrieveLatest(name, before)`                    | Use IDs: `client.datapoints.retrieveLatest([{id, before}])`                                        |
| `client.datapoints.retrieveCSV(...)`                             | Removed                                                                                            |
| `sdk.Datapoints.delete(name, timestamp)`                         | Use IDs: `client.datapoints.delete([{id, inclusiveBegin: timestamp, exclusiveEnd: timestamp}])`    |
| `sdk.Datapoints.deleteRange(name, inclusiveBegin, exclusiveEnd)` | Use IDs: `client.datapoints.delete([{id, inclusiveBegin, exclusiveEnd}])`                          |
| **EVENTS**                                                       |
| `sdk.Events.create([...])`                                       | `client.events.create([...])`                                                                      |
| `sdk.Events.retrieve(id)`                                        | `client.events.retrieve([{id}])`                                                                   |
| `sdk.Events.retrieveMultiple([123, 456])`                        | `client.events.retrieve([{id: 123}, {id: 456}])`                                                   |
| `sdk.Events.update([{id, ...changes}])`                          | `client.events.update([{id, update: changes}])`                                                    |
| `sdk.Events.delete([id])`                                        | `client.events.delete([{id}])`                                                                     |
| `sdk.Events.list({limit: 20})`                                   | `client.events.list().autoPagingToArray({limit: 20})`                                              |
| `sdk.Events.search({name: 'abc'})`                               | `client.events.search({search: {name: 'abc'}})`                                                    |
| **FILES**                                                        |
| `sdk.Files.upload(fileMetadata, params)`                         | `client.files.upload(fileMetadata, fileContent?, overwrite?, waitUntilAcknowledged?)`              |
| `sdk.Files.download(id)`                                         | `client.files.getDownloadUrls([{id}])`                                                             |
| `sdk.Files.retrieveMetadata(id)`                                 | `client.files.retrieve([{id}])`                                                                    |
| `sdk.Files.retrieveMultipleMetadata([123, 456])`                 | `client.files.retrieve([{id: 123}, {id: 456}])`                                                    |
| `sdk.Files.updateMetadata(id, changes)`                          | `client.files.update([{id, update: changes}])`                                                     |
| `sdk.Files.updateMultipleMetadata([id, ...changes])`             | `client.files.update([{id, update: changes}])`                                                     |
| `sdk.Files.delete([id])`                                         | `client.files.delete([{id}])`                                                                      |
| `sdk.Files.list({limit: 20})`                                    | `client.files.list().autoPagingToArray({limit: 20})`                                               |
| `sdk.Files.search({name: 'abc'})`                                | `client.files.search({search: {name: 'abc'}})`                                                     |
| `sdk.Files.replaceMetadata(...)`                                 | Removed                                                                                            |
| **3D**                                                           |
| `sdk.ThreeD.createAssetMappings(modelId, revisionId, mappings)`  | `client.assetMappings3D.create(modelId, revisionId, mappings)`                                     |
| `sdk.ThreeD.createModels([name])`                                | `client.models3D.create([{name}])`                                                                 |
| `sdk.ThreeD.createRevisions(modelId, revisions)`                 | `client.revisions3D.create(modelId, revisions)`                                                    |
| `sdk.ThreeD.retrieveFile(fileId, responseType)`                  | `client.files3D.retrieve(fileId)`                                                                  |
| `sdk.ThreeD.retrieveModel(modelId)`                              | `client.models3D.retrieve(modelId)`                                                                |
| `sdk.ThreeD.retrieveRevision(modelId, revisionId)`               | `client.models3D.retrieve(modelId, revisionId)`                                                    |
| `sdk.ThreeD.updateModels({id, name})`                            | `client.models3D.update([{id, update: {name}])`                                                    |
| `sdk.ThreeD.updateRevisions(modelId, {id, ...changes})`          | `client.revisions3D.update(modelId, [{id, update: changes])`                                       |
| `sdk.ThreeD.updateRevisionThumbnail(...)`                        | `client.revisions3D.updateThumbnail(modelId, revisionId, fileId)`                                  |
| `sdk.ThreeD.deleteModels([id])`                                  | `client.models3D.delete([{id}])`                                                                   |
| `sdk.ThreeD.deleteRevisions(modelId, [id])`                      | `client.revisions3D.delete(modelId, [{id}])`                                                       |
| `sdk.ThreeD.deleteAssetMappings(modelId, revisionId, mappings)`  | `client.assetMappings3D.delete(modelId, revisionId, mappings)`                                     |
| `sdk.ThreeD.listAssetMappings(modelId, revisionId, params)`      | `client.assetMappings3D.list(modelId, revisionId, params).autoPagingToArray()`                     |
| `sdk.ThreeD.listModels(params)`                                  | `client.models3D.list(params).autoPagingToArray()`                                                 |
| `sdk.ThreeD.listNodes(modelId, revisionId, params)`              | `client.revisions3D.list3DNodes(modelId, revisionId, params).autoPagingToArray()`                  |
| `sdk.ThreeD.listNodeAncestors(modelId, revisionId, params)`      | `client.revisions3D.list3DNodeAncestors(modelId, revisionId, nodeId, params).autoPagingToArray()`  |
| `sdk.ThreeD.listRevisions(modelId, params)`                      | `client.revisions3D.list(modelId, params).autoPagingToArray()`                                     |
| `sdk.ThreeD.listSectors(modelId, revisionId, params)`            | `client.viewer3D.listRevealSectors3D(modelId, revisionId, params).autoPagingToArray()`             |
