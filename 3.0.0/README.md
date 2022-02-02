# TileJSON 3.0.0

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://tools.ietf.org/html/rfc2119).

**Table of contents**

1. [Purpose](#1-purpose)
1. [File format](#2-file-format)
1. [Structure](#3-structure)
   1. [tilejson](#31-tilejson)
   1. [tiles](#32-tiles)
   1. [vector_layers](#33-vector_layers)
   1. [attribution](#34-attribution)
   1. [bounds](#35-bounds)
   1. [center](#36-center)
   1. [data](#37-data)
   1. [description](#38-description)
   1. [fillzoom](#39-fillzoom)
   1. [grids](#310-grids)
   1. [legend](#311-legend)
   1. [maxzoom](#312-maxzoom)
   1. [minzoom](#313-minzoom)
   1. [name](#314-name)
   1. [scheme](#315-scheme)
   1. [template](#316-template)
   1. [version](#317-version)
1. [Examples](#4-examples)
1. [Caching](#5-caching)

# 1. Purpose

This specification attempts to create a standard for representing metadata about multiple types of web-based map layers, to aid clients in configuration and browsing.

# 2. File Format

TileJSON manifest files use the JSON format as described in [RFC 8259](https://tools.ietf.org/html/rfc8259).

# 3. Structure

The following describes the structure of a TileJSON object. Implementations MUST treat unknown keys as if they weren't present. However, implementations MUST expose unknown key value pairs so users can optionally handle these keys. Implementations MUST treat invalid values for keys as if they weren’t present. If the the key is optional and the value is invalid, the default value MAY be applied. If the key is required, implementations MUST treat the entire TileJSON manifest file as invalid and refuse operation.

*The word "implementation" in the following sections refers to services or tools that serve, generate, or validate TileJSON objects.*

## 3.1 `tilejson`

REQUIRED. String.

A semver.org style version number as a string. Describes the version of the TileJSON spec that is implemented by this JSON object.

```JSON
{
  "tilejson": "3.0.0"
}
```

## 3.2 `tiles`

REQUIRED. Array<String>.

An array of tile endpoints. {z}, {x} and {y}, if present, are replaced with the corresponding integers. If multiple endpoints are specified, clients may use any combination of endpoints. All endpoint urls MUST be absolute. All endpoints MUST return the same content for the same URL. The array MUST contain at least one endpoint. The tile extension is NOT limited to any particular format. Some of the more popular are: mvt, vector.pbf, png, webp, and jpg.

```JSON
{
  "tiles": [
    "http://localhost:8888/admin/1.0.0/world-light,broadband/{z}/{x}/{y}.mvt"
  ]
}
```

## 3.3 `vector_layers`

REQUIRED. Array<Object>.

An array of objects. Each object describes one layer of vector tile data. A `vector_layer` object MUST contain the `id` and `fields` keys, and MAY contain the `description`, `minzoom`, or `maxzoom` keys. An implemenntation MAY include arbitrary keys in the object outside of those defined in this specification.

Note: When describinng a set of raster tiles or other tile format that does not have a "layers" concept (i.e. `"format": "jpeg"`), the `vector_layers` key is not required.

#### 3.3.1 `id`

REQUIRED. String.

A string value representing the the layer id. For added context, this is referred to as the `name` of the layer in the [Mapbox Vector Tile spec](https://github.com/mapbox/vector-tile-spec/tree/master/2.1#41-layers).

#### 3.3.2 `fields`

REQUIRED. Object.

An object whose keys and values are the names and descriptions of attributes available in this layer. Each value (description) MUST be a string that describes the underlying data. If no fields are present, the `fields` key MUST be an empty object.

#### 3.3.3 `description`

OPTIONAL. String.

A string representing a human-readable description of the entire layer's contents.

#### 3.3.4 `minzoom` and `maxzoom`

OPTIONAL. Integer.

An integer representing the lowest/highest zoom level whose tiles this layer appears in. `minzoom` MUST be greater than or equal to the set of tiles' `minzoom`. `maxzoom` MUST be less than or equal to the set of tiles' `maxzoom`.

These keys are used to describe the situation where different sets of vector layers appear in different zoom levels of the same set of tiles, for example in a case where a "minor roads" layer is only present at high zoom levels.

```JSON
{
  "vector_layers": [
    {
      "id": "roads",
      "description": "Roads and their attributes",
      "minzoom": 2,
      "maxzoom": 16,
      "fields": {
        "type": "One of: trunk, primary, secondary",
        "lanes": "Number",
        "name": "String",
        "sidewalks": "Boolean"
      }
    },
    {
      "id": "countries",
      "description": "Admin 0 (country) boundaries",
      "minzoom": 0,
      "maxzoom": 16,
      "fields": {
        "iso": "ISO 3166-1 Alpha-2 code",
        "name": "English name of the country",
        "name_ar": "Arabic name of the country"
      }
    },
    {
      "id": "buildings",
      "description": "A layer with an empty fields object",
      "fields": {}
    }
  ]
}
```

## 3.4 `attribution`

OPTIONAL. String. Default: null.

Contains an attribution to be displayed when the map is shown to a user. Implementations MAY decide to treat this as HTML or literal text. For security reasons, make absolutely sure that this content can't be abused as a vector for XSS or beacon tracking.

```JSON
{
  "attribution": "<a href='http://openstreetmap.org'>OSM contributors</a>"
}
```

## 3.5 `bounds`

OPTIONAL. Array<Number>. Default: [ -180, -85.05112877980659, 180, 85.0511287798066 ] (xyz-compliant tile bounds)

The maximum extent of available map tiles. Bounds MUST define an area covered by all zoom levels. The bounds are represented in WGS 84 latitude and longitude values, in the order left, bottom, right, top. Values may be integers or floating point numbers. The minimum/maximum values for longitude and latitude are -180/180 and -90/90 respectively. Bounds MUST NOT "wrap" around the ante-meridian. If bounds are not present, the default value MAY assume the set of tiles is globally distributed.

```JSON
{
  "bounds": [ -180, -85.05112877980659, 180, 85.0511287798066 ]
}
```

Bounds where longitude values are the same, and latitude values are the same, are considered valid. This case typically represents a single point geometry in the entire tileset. For example:

```JSON
{
  "bounds": [-122.34, 47.65, -122.34, 47.65]
}
```

## 3.6 `center`

OPTIONAL. Array<Number>. Default: null.

The first value is the longitude, the second is latitude (both in WGS:84 values), the third value is the zoom level as an integer. Longitude and latitude MUST be within the specified bounds. The zoom level MUST be between minzoom and maxzoom. Implementations MAY use this center value to set the default location. If the value is null, implementations MAY use their own algorithm for determining a default location.

```JSON
{
  "center": [ -76.275329586789, 39.153492567373, 8 ]
}
```

## 3.7 `data`

OPTIONAL. Array<String>. Default: [].

An array of data files in GeoJSON format. {z}, {x} and {y}, if present, are replaced with the corresponding integers. If multiple endpoints are specified, clients may use any combination of endpoints. All endpoints MUST return the same content for the same URL. If the array doesn't contain any entries, then no data is present in the map. *This field is for overlaying GeoJSON data on tiled raster maps and is generally [no longer used](https://github.com/mapbox/tilejson-spec/pull/43) for GL-based maps.*

```JSON
{
  "data": [
    "http://www.example.com/admin/data.geojson"
  ]
}
```

## 3.8 `description`

OPTIONAL. String. Default: null.

A text description of the set of tiles. The description can contain any valid unicode character as described by the JSON specification [RFC 8259](https://tools.ietf.org/html/rfc8259).

```JSON
{
  "description": "Highways, roads, and vehicular tracks derived from OpenStreetMap."
}
```

## 3.9 `fillzoom`

OPTIONAL. Integer. Default: null.

An integer specifying the zoom level from which to generate overzoomed tiles. Implementations MAY generate overzoomed tiles from parent tiles if the requested zoom level does not exist. In most cases, overzoomed tiles are generated from the maximum zoom level of the set of tiles. If fillzoom is specified, the overzoomed tile MAY be generated from the fillzoom level.

For example, in a set of tiles with maxzoom 10 and _no_ fillzoom specified, a request for a z11 tile will use the z10 parent tiles to generate the new, overzoomed z11 tile. If the same TileJSON object had fillzoom specified at z7, a request for a z11 tile would use the z7 tile instead of z10.

While TileJSON may specify rules for overzooming tiles, it is ultimately up to the tile serving client or renderer to implement overzooming.

```JSON
{
  "fillzoom": 7
}
```

## 3.10 `grids`

OPTIONAL. Array<String>. Default: [].

An array of interactivity endpoints. {z}, {x} and {y}, if present, are replaced with the corresponding integers. If multiple endpoints are specified, clients may use any combination of endpoints. All endpoints MUST return the same content for the same URL. If the array doesn't contain any entries, UTF-Grid interactivity is not supported for this set of tiles. See https://github.com/mapbox/utfgrid-spec/tree/master/1.2 for the interactivity specification.

*Note: UTF-Grid interactivity predates GL-based map rendering and interaction. Map interactivity is now generally defined outside of the TileJSON specification and is dependent on the tile rendering library's features.*

```JSON
{
  "grids": [
    "http://www.example.com/earthsea/1.0.0/{z}/{x}/{y}.grid.json"
  ]
}
```

## 3.11 `legend`

OPTIONAL. String. Default: null.

Contains a legend to be displayed with the map. Implementations MAY decide to treat this as HTML or literal text. For security reasons, make absolutely sure that this field can't be abused as a vector for XSS or beacon tracking.

```JSON
{
  "legend": "Dangerous zones are red, safe zones are green"
}
```

## 3.12 `maxzoom`

OPTIONAL. Integer. Default: 30.

An integer specifying the maximum zoom level. MUST be in range: 0 <= minzoom <= maxzoom <= 30. A client or server MAY request tiles outside of the zoom range, but the availability of these tiles is dependent on how the the tile server or renderer handles the request (such as overzooming tiles).

```JSON
{
  "maxzoom": 11
}
```

## 3.13 `minzoom`

OPTIONAL. Integer. Default: 0.

An integer specifying the minimum zoom level. MUST be in range: 0 <= minzoom <= maxzoom <= 30.

```JSON
{
  "minzoom": 0
}
```

## 3.14 `name`

OPTIONAL. String. Default: null.

A name describing the set of tiles. The name can contain any legal character. Implementations SHOULD NOT interpret the name as HTML.

```JSON
{
  "name": "Earthsea v2"
}
```

## 3.15 `scheme`

OPTIONAL. String. Default: "xyz".

Either "xyz" or "tms". Influences the y direction of the tile coordinates. The global-mercator (aka Spherical Mercator) profile is assumed.

```JSON
{
  "scheme": "xyz"
}
```

## 3.16 `template`

OPTIONAL. String. Default: null.

Contains a mustache template to be used to format data from grids for interaction. See https://github.com/mapbox/utfgrid-spec/tree/master/1.2 for the interactivity specification.

```JSON
{
  "template": "{{#__teaser__}}{{NAME}}{{/__teaser__}}"
}
```

## 3.17 `version`

OPTIONAL. String. Default: "1.0.0".

A [semver.org](https://semver.org) style version number of the tiles. When changes across tiles are introduced the minor version MUST change. This may lead to cut off labels. Therefore, implementors can decide to clean their cache when the minor version changes. Changes to the patch level MUST only have changes to tiles that are contained within one tile. When tiles change significantly, such as updating a vector tile layer name, the major version MUST be increased. Implementations MUST NOT use tiles with different major versions.

```JSON
{
  "version": "1.0.0"
}
```

# 4. Examples

Examples can be found in the [examples directory](./example).

# 5. Caching

Clients MAY cache files retrieved from a remote server. When implementations decide to perform caching, they MUST honor valid cache control HTTP headers as defined in the HTTP specification for both tile images and the TileJSON manifest file.
