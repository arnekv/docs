---
pagination_next: null
pagination_prev: null
title: Migration guide
---

# Migrate from the query service to the seismic service

:::info Limited availability
The features described in this section are currently only available to selected customers.
:::

The guide helps you migrate from the **query v0 service** to the **seismic v1 service**. It provides translations for the **v0 API** methods. The document suggests solutions if there's no direct translation or methods are deprecated.

These concepts have changed in the seismic v1 service:

- **SeismicStore and Seismic objects**. The [`SeismicStore`](/dev/guides/sdk/seismic/api/v1/#seismicstore) is an object that represents an ingested SEG-Y file but isn't directly available as part of the entitlement. The [`Seismic`](/dev/guides/sdk/seismic/api/v1/#seismic) object is a cutout of a `SeismicStore` and composes an entitlement. Queries no longer reference files directly and use either the `Seismic` or the `SeismicStore` objects instead.

- **Search, Get, and List queries**. The Seismic API v1 combines the **Search**, **Get**, and **List** metadata queries into a single generic **Search** query. This query accepts a search specification. For example, searching by an identifier or a unique name will show the results of the **Get** query, and an empty search will behave like the **List** query.

- **`StreamTraces` endpoint**. The Seismic API v1 combines many trace access endpoints into a single, more flexible [`com.cognite.seismic.v1.SeismicService.StreamTraces`](/dev/guides/sdk/seismic/api/v1/#streamtracesrequest) endpoint. By changing the query parameters slightly, you can produce similar results to many of the methods the endpoint replaces.

:::info
To learn more about the Seismic API v1, see the [documentation](/dev/guides/sdk/seismic/api/v1/).
:::

# Metadata queries and payload examples

Metadata queries consist of `com.cognite.seismic.Query.` + `MethodName` = `com.cognite.seismic.Query.MethodName`, for example, `com.cognite.seismic.Query.GetSurvey`. See below for [changed](#changed-methods) and [deprecated](#deprecated-methods) methods.

## Changed methods

<!-- vale off -->

### GetSurvey

Use [`com.cognite.seismic.v1.SeismicService.SearchSurveys`](/dev/guides/sdk/seismic/api/v1/#seismicapi).

#### Payload example

```json title="v0"
com.cognite.seismic.Query/GetSurvey
{
  "survey": {
    "name": "some_name"
  }
}
```

```json title="v1"
com.cognite.seismic.v1.SeismicAPI/SearchSurveys
{
  "surveys": {
    "name": "some_name"
  }
}
```

### ListSurveys

Use [`com.cognite.seismic.v1.SeismicService.SearchSurveys`](/dev/guides/sdk/seismic/api/v1/#seismicapi) without a [`surveys`](/dev/guides/sdk/seismic/api/v1/#searchsurveysrequest) search specification.

#### Payload example

```json title="v0"
com.cognite.seismic.Query/ListSurveys
{
  "include_metadata": true
}
```

```json title="v1"
com.cognite.seismic.v1.SeismicAPI/SearchSurveys
{
  "include_metadata": true
}
```

### ListFiles

Use [`com.cognite.seismic.v1.SeismicService.SearchFiles`](/dev/guides/sdk/seismic/api/v1/#seismicapi) without the [`spec`](/dev/guides/sdk/seismic/api/v1/#searchfilesrequest) search specification.

### SearchSurveys

Use [`com.cognite.seismic.v1.SeismicService.SearchSurveys`](/dev/guides/sdk/seismic/api/v1/#seismicapi).

### GetFile

Use [`com.cognite.seismic.v1.SeismicService.SearchFiles`](/dev/guides/sdk/seismic/api/v1/#seismicapi) with the [`spec`](/dev/guides/sdk/seismic/api/v1/#searchfilesrequest) containing the [`identifier`](/dev/guides/sdk/seismic/api/v1/#searchspec) for the file.

#### Payload example

```json title="v0"
com.cognite.seismic.Query/GetFile
{
  "file": {
    "name": "some_name"
  }
}
```

```json title="v1"
com.cognite.seismic.v1.SeismicAPI/SearchFiles
{
  "spec": {
    "name": "some_name"
  }
}
```

### GetBinaryHeader

Fetch the original SEG-Y binary headers from [`com.cognite.seismic.v1.SeismicService.SearchSeismicStores`](/dev/guides/sdk/seismic/api/v1/#seismicapi) by setting [`include_headers`](/dev/guides/sdk/seismic/api/v1/#searchseismicstoresrequest) to `true`.

#### Payload example

```json title="v0"
com.cognite.seismic.Query/GetBinaryHeader
{
  "file": {
    "name": "some_name"
  }
}
```

```json title="v1"
com.cognite.seismic.v1.SeismicAPI/SearchSeismicStores
{
  "file": {
    "name": "some_name"
  },
  "include_headers": true
}
```

<!-- vale on -->

:::info Info
[`Seismic`](/dev/guides/sdk/seismic/api/v1/#com.cognite.seismic.v1.Seismic) objects also have binary headers. By default, the objects inherit the headers from the original file, but data managers can override them. You can fetch the headers from [`com.cognite.seismic.v1.SeismicService.SearchSeismics`](/dev/guides/sdk/seismic/api/v1/#seismicapi).
:::

<!-- vale off -->

#### Payload example

```json title="v1"
com.cognite.seismic.v1.SeismicAPI/SearchSeismics
{
  "seismic_store": {
    "name": "some_name"
  },
  "include_binary_header": true
}
```

### GetTextHeader

Fetch the original SEG-Y text headers from [`com.cognite.seismic.v1.SeismicService.SearchSeismicStores`](/dev/guides/sdk/seismic/api/v1/#seismicapi) by setting [`include_headers`](/dev/guides/sdk/seismic/api/v1/#searchseismicstoresrequest) to `true`.

#### Payload example

```json title="v0"
com.cognite.seismic.Query/GetTextHeader
{
  "file": {
    "name": "some_name"
  }
}
```

```json title="v1"
com.cognite.seismic.v1.SeismicAPI/SearchSeismicStores
{
  "seismic_store": {
    "name": "some_name"
  },
  "include_headers": true
}
```

<!-- vale on -->

:::info Info
[`Seismic`](/dev/guides/sdk/seismic/api/v1/#com.cognite.seismic.v1.Seismic) objects also have binary headers. By default, the objects inherit the headers from the original file, but data managers can override them. You can fetch the headers from [`com.cognite.seismic.v1.SeismicService.SearchSeismics`](/dev/guides/sdk/seismic/api/v1/#seismicapi).
:::

<!-- vale off -->

#### Payload example

```json title="v1"
com.cognite.seismic.v1.SeismicAPI/SearchSeismics
{
  "seismic_store": {
    "name": "some_name"
  },
  "include_text_header": true
}
```

### GetFileDataCoverage

Use [`com.cognite.seismic.v1.SeismicService.SearchSeismicStores`](/dev/guides/sdk/seismic/api/v1/#seismicapi) specifying [`coverage`](/dev/guides/sdk/seismic/api/v1/#searchseismicstoresrequest) with the selected [`format`](/dev/guides/sdk/seismic/api/v1/#coveragespec).

#### Payload example

```json title="v0"
com.cognite.seismic.Query/GetFileDataCoverage
{
  "file": {
    "name": "some_name"
  },
  "in_wkt": true
}
```

```json title="v1"
com.cognite.seismic.v1.SeismicAPI/SearchSeismicStores
{
  "file": {
    "name": "some_name"
  },
  "coverage": {
    "format": 0 // 0 indicates wkt, 1 indicates geojson
  }
}
```

<!-- vale on -->

:::info Info
[`Seismic`](/dev/guides/sdk/seismic/api/v1/#com.cognite.seismic.v1.Seismic) objects also have coverage that you can fetch from [`com.cognite.seismic.v1.SeismicService.SearchSeismics`](/dev/guides/sdk/seismic/api/v1/#seismicapi).
:::

<!-- vale off -->

#### Payload example

```json title="v1"
com.cognite.seismic.v1.SeismicAPI/SearchSeismics
{
  "seismic_store": {
    "name": "some_name"
  },
  "coverage": {
    "format": 0
  }
}
```

### GetFileLineRange

Use [`com.cognite.seismic.v1.SeismicService.GetTraceBounds`](/dev/guides/sdk/seismic/api/v1/#seismicapi).

<!-- vale on -->

:::info Note
In v1, the query is in terms of [`Seismic`](/dev/guides/sdk/seismic/api/v1/#com.cognite.seismic.v1.Seismic) or [`SeismicStore`](/dev/guides/sdk/seismic/api/v1/#com.cognite.seismic.v1.SeismicStore) objects. In the example below, assume that the `SeismicStore` ID for the ingested file of interest is known.
:::

<!-- vale off -->

#### Payload example

```json title="v0"
com.cognite.seismic.Query/GetFileLineRange
{
  "file": {
    "name": "some_name"
  }
}
```

```json title="v1"
com.cognite.seismic.v1.SeismicAPI/GetTraceBounds
{
  "seismic_store_id": 1234
}
```

### GetCrosslinesByInline

Use [`com.cognite.seismic.v1.SeismicService.SearchSeismicStores`](/dev/guides/sdk/seismic/api/v1/#seismicapi) with [`include_extent`](/dev/guides/sdk/seismic/api/v1/#searchseismicstoresrequest) set to `true`. The method returns all lines that contain the _inline_.

#### Payload example

```json title="v0"
com.cognite.seismic.Query/GetFileLineRange
{
  "file": {
    "name": "some_name"
  }
}
```

```json title="v1"
com.cognite.seismic.v1.SeismicAPI/SearchSeismicStores
{
  "file": {
    "name": "some_name"
  },
  "include_extent": true
}
```

<!-- vale on -->

:::info Info
[`Seismic`](/dev/guides/sdk/seismic/api/v1/#com.cognite.seismic.v1.Seismic) objects also have the line range that you can fetch from [`com.cognite.seismic.v1.SeismicService.SearchSeismics`](/dev/guides/sdk/seismic/api/v1/#seismicapi).
:::

<!-- vale off -->

#### Payload example

```json title="v1"
com.cognite.seismic.v1.SeismicAPI/SearchSeismics
{
  "seismic": {
    "id": "1234"
  },
  "include_line_range": true
}
```

### GetInlinesByCrossline

See [`com.cognite.seismic.Query.GetCrosslinesByInline`](#getcrosslinesbyinline)

### GetTraceByCoordinates

<!-- vale on -->

To get the closest trace to a given coordinate, use [`com.cognite.seismic.v1.SeismicService.StreamTraces`](/dev/guides/sdk/seismic/api/v1/#seismicapi) and specify the [`geometry`](/dev/guides/sdk/seismic/api/v1/#streamtracesrequest) with [`interpolation_method`](/dev/guides/sdk/seismic/api/v1/#geometryfilter) set to [`NEAREST_TRACE`](/dev/guides/sdk/seismic/api/v1/#interpolationmethod), and a [`geometry`](/dev/guides/sdk/seismic/api/v1/#geometry) containing the coordinate of interest.

<!-- vale off -->

#### Payload example

```json title="v0"
com.cognite.seismic.Query/GetTraceByCoordinates
{
  "file": {
    "name": "some_name",
  },
  "coordinates": {
    "x": 10,
    "y": 20
  }
}
```

```json title="v1"
com.cognite.seismic.v1.SeismicAPI/StreamTraces
{
  "seismic": {
    "id": "1234"
  },
  "geometry": {
    "geometry": {
      "wkt": {
        "geometry": "POINT(10 20)"
      }
    },
    "interpolation_method": 0 // 0 indicates NEAREST_TRACE, 1 indicates INVERSE_DISTANCE_WEIGHTING
  }
}
```

### GetSliceByGeometry

Use [`com.cognite.seismic.v1.SeismicService.StreamTraces`](/dev/guides/sdk/seismic/api/v1/#seismicapi) passing in the desired [`geometry`](/dev/guides/sdk/seismic/api/v1/#streamtracesrequest).

See [`com.cognite.seismic.Query.GetTraceByCoordinates`](#gettracebycoordinates).

### GetCubeByGeometry

Use [`com.cognite.seismic.v1.SeismicService.StreamTraces`](/dev/guides/sdk/seismic/api/v1/#seismicapi) passing in the desired [`geometry`](/dev/guides/sdk/seismic/api/v1/#streamtracesrequest).

See [`com.cognite.seismic.Query.GetTraceByCoordinates`](#gettracebycoordinates).

### GetSegYFile

Use [`com.cognite.seismic.v1.SeismicService.GetSegYFile`](/dev/guides/sdk/seismic/api/v1/#seismicapi) instead.

### GetTimeSliceByGeometry

<!-- vale on -->

Use [`com.cognite.seismic.v1.SeismicService.StreamTraces`](/dev/guides/sdk/seismic/api/v1/#seismicapi) passing in the desired [`geometry`](/dev/guides/sdk/seismic/api/v1/#streamtracesrequest) with a [`z_range`](/dev/guides/sdk/seismic/api/v1/#streamtracesrequest) containing the depth of interest and a desired [`geometry`](/dev/guides/sdk/seismic/api/v1/#com.cognite.seismic.v1.GeometryFilter).

<!-- vale off -->

See [`com.cognite.seismic.Query.GetSliceByGeometry`](#getslicebygeometry).

#### Payload example

```json title="v0"
com.cognite.seismic.Query/GetTimeSliceByGeometry
{
  "file": {
    "name": "some_file"
  },
  "geometry": {
    "wkt": {
      "geometry": "POLYGON ((30 10, 40 40, 20 40, 10 20, 30 10))"
    }
  },
  "z": 20
}
```

```json title="v1"
com.cognite.seismic.v1.SeismicAPI/StreamTraces
{
  "seismic": {
    "id": "1234"
  },
  "geometry": {
    "geometry": {
      "wkt": {
        "geometry": "POLYGON ((30 10, 40 40, 20 40, 10 20, 30 10))"
      }
    }
  },
  "z_range": {
    "min": 20,
    "max": 20
  }
}
```

### Query.GetVolume

Use [`com.cognite.seismic.v1.SeismicService.StreamTraces`](/dev/guides/sdk/seismic/api/v1/#seismicapi) instead.

## Deprecated methods

### GetSliceByLine

Use [`com.cognite.seismic.v1.SeismicService.StreamTraces`](/dev/guides/sdk/seismic/api/v1/#seismicapi) instead.

### GetCubeByLines

Use [`com.cognite.seismic.v1.SeismicService.StreamTraces`](/dev/guides/sdk/seismic/api/v1/#seismicapi) instead.

### GetSlabByLines

See [`com.cognite.seismic.Query.GetTimeSliceByGeometry`](#gettimeslicebygeometry).

<!-- vale on -->

## Next steps

View the [documentation](/dev/guides/sdk/seismic/api/v1/) to learn more about the Seismic API v1.
