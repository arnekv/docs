---
pagination_next: null
pagination_prev: null
---

# RAW

In Cognite Data Fusion (CDF), the RAW resource type stores unstructured data. The RAW database and tables hold the source data in its _original form_ to reduce source system queries for the same data for different use cases and minimize the data extractors' logic. This makes it easy to re-run transformations on data in the cloud.

Alternatively, you can transform the data in your cloud and bypass CDF RAW to integrate the data directly into the CDF data model.

## About RAW data

Use the RAW data to spot anomalies in your tabular data or identify which transformations you need to do on your data before ingesting it into the CDF data model. Navigate to [Manage staged data](https://docs.cognite.com/cdf/integration/guides/extraction/raw_explorer) in the CDF portal application to view the ingested tabular data in a table or as a data profiling report in the **RAW explorer**.

A CDF project can have a variable number of RAW databases with a variable number of tables with a variable number of key-value objects. You can query the keys using the [RAW API](/api#tag/Raw) and post a maximum of 1000 databases per request.

## Primary row key

When you insert rows in a RAW table, you must set a primary row key that only contains unique values. You can't change this key when it's set. In the **RAW explorer**, you can select **Generate a new key column** to generate a unique key per row.

:::tip TIP
If you're unsure which primary key to use and want to simulate different scenarios, upload the same file to different tables using separate tabs in your browser.
:::

:::caution Caution
You may risk losing data if you use a non-unique column as the primary key.
:::
