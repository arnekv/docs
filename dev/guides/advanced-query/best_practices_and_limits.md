---
pagination_next: null
title: Best practices and limits
---

# Best practices and limits

This page summarizes the best development practices about existing limits and restrictions for advanced queries.

## Pagination and partitions

CDF supports a throughput of 1,000 items per request. For increased concurrency, use the appropriate number of partitions required, up to a maximum of 10. 

For more information, see [Pagination](https://developer.cognite.com/dev/concepts/pagination/).

## Rate and concurrency limiting

CDF enforces rate and concurrency limits to provide service protection and guarantee allocated capacity and fair usage policy per project.

For more information, see [Request throttling](https://developer.cognite.com/dev/concepts/request_throttling/).

## Limits and restrictions

Search, filtering, and aggregation requests are computationally expensive compared to CRUD operations. Hence, they are subject to lower resource limits. 

For more detailed information on rate and concurrency limits, see below:

- [Assets](https://developer.cognite.com/api/#tag/Assets)
- [Events](https://developer.cognite.com/api/#tag/Events)
- [Aggregate time series](https://developer.cognite.com/api/#tag/Time-series/operation/aggregateTimeSeries)
- [Aggregate sequences](https://developer.cognite.com/api/#tag/Sequences/operation/aggregateSequences)
- [Search for documents](https://developer.cognite.com/api/#tag/Documents/operation/documentsSearch/)

