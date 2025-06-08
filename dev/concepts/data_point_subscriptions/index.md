---
pagination_next: null
pagination_prev: null
---

# Data point subscriptions
Cognite Data Fusion provides a **data point subscription** feature that allows you to **subscribe** to time series updates in **real-time**.

:::tip
For more information on how to work with Data point subscriptions, see [Data point subscriptions API documentation](https://developer.cognite.com/api/#tag/Data-point-subscriptions).
:::

## How data point subscriptions differ from time series API
Subscriptions will provide you the **changes** to a given set of time series, while the time series API will give you the **current state** of the time series.

This is especially important if you ingest data out of order or delete data point ranges. Subscriptions can retrieve all changes, no
matter the time stamp of the data points, while the time series API requires you to specify a time range to query.

Another significant difference is that subscriptions can query data from a large amount of time series,
while the time series API is limited to 100 time series per request.

### Use cases for data point subscriptions
Consider using data point subscriptions in the following scenarios:

- When you need to listen to updates that arrive out of order.
- When you want to monitor changes across a large set of time series.
- When you require a constantly updated local copy of time series data.
- When you aim to keep track of changes in a dynamic set of time series.

### Limitations of data point subscriptions
However, there are several limitations to keep in mind when using data point subscriptions:

- Subscriptions tend to be slower than regular endpoints for time series data points and aggregates, especially when data in small batches gets ingested, deleted, or overwritten.
- Subscriptions only maintain order based on ingestion time; you cannot filter by time series or data point timestamps.
- Subscriptions can return data that was subsequently deleted or overwritten.
- Subscriptions do not store client state on the server; it is necessary to track progress locally.

## Set up a subscription
The first step is to decide on what you want to subscribe to.
You can subscribe to data points for a specific set of time series or a set of time series that match a filter.

### Subscribe to a set of time series
To subscribe to a set of time series, you need to provide a list of external IDs. You can update this list anytime, and it will immediately take effect for the subscription.

### Create a filter subscription
To create a filter subscription, you need to provide a search filter. This filter will be evaluated against all time series in the project. Time series that match the filter will automatically be added to the subscription. Conversely, time series that no longer match the filter or have been deleted will be removed from the subscription. Note that there may be a short delay between the time a series is created or updated to match the filter and the time it is added to the subscription.

It's important to note that only data points ingested into a time series after it becomes a member of the subscription will be included in the subscription. Therefore, data points ingested immediately after the creation of a time series might be omitted from the subscription.

With a filter subscription, you can subscribe to all time series that match specific criteria, such as a particular asset, data set, metadata value prefix, or a combination of these. The filter language is similar to that used in the list endpoint of the time series API.

## Query the subscription
Once you have created a subscription, we will start recording changes to its time series, and you can query these changes through the `/data/list` endpoint.

The query will return the following information:
- A **list of data point changes** that includes either data point upserts or data point range deletes for a given time series.
- A **list of subscription changes** that includes the list of time series added to or removed from the subscription. When you create a subscription, all time series in the subscription will be marked as added.
- A **cursor** to query the next page of results.
- A **hasNext** field indicates if more results are available.

### Long polling

The `/data/list` endpoint will by default use _long polling_.
- If there are any pending changes, the API will respond immediately.
- If there are no pending changes, the API will wait for changes for up to 5 seconds.

The long polling period can be changed by setting the `pollTimeoutSeconds` property in the request body.
Long polling can be disabled by setting the property to 0.

### Cursor
If you consider a subscription a stream of data, the cursor is the position in the stream where you are currently reading. It is very similar to an offset in Kafka.

The cursor is a string that is opaque to the user and keeps track of the position in the stream. The user must store the cursor. It is not stored in Cognite Data Fusion. It does not expire, and you may use it multiple times.

#### Initializing the cursor
When you create the subscription, the cursor gets initialized to the creation time of the subscription. The first query will return all the changes since the subscription was created.

To start from a different point in time, you can specify a start time in the query (e.g. "1d-ago"), and the cursor will be initialized to approximately that time.

### HasNext
The hasNext field indicates if there are more results available. If true, you can query the next results page using the next cursor.
If it is false, no more results are available, so you should wait a little while for more data to come in and try again using the next cursor.

## Retention
The retention time for data point subscriptions is 7 days. Data older than 7 days will not be returned in the query.
This applies both to data points and subscription changes.

## How to create and maintain a local copy of your CDF time series data
A subscription will provide you with changes to a set of time series. Depending on the change type,
you apply changes to the local copy in one of three ways:
1. If the subscription says a time series has been deleted, delete it from your local copy.
2. If the subscription says a time series has been added, create it in your local copy and then query the time series API to get the current state of the time series.
3. If the subscription says data points have been updated, apply the updates to your local copy in the same order as they appear in the subscription.

Each change has to be applied in full, in the order they appear in the subscription.

Every time you have reached the end of the subscription stream, your local copy will be up-to-date.

Make sure that you never fall more than 7 days behind, or you will have to re-create your local copy of all time series in the subscription, and then apply subscription changes from a time before the re-creation.

Also, if a time series is marked as both deleted and added, you may have to check which is correct.

## Advanced usage
### Partitions
Subscriptions with many high-volume time series will return a lot of data. If this data is too much for a single client to handle,
you can use partitions to split the subscription data into multiple partitions (streams).

When you create or update a subscription, each time series will be assigned to **exactly one** partition. For instance,
a subscription with 6 time series could be divided into 3 partitions as follows: (1,6), (4), (2,3,5).
A partition can contain zero time series and thus no data.

Each partition will have its cursor, and you query each partition independently.
