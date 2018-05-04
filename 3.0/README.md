# TileJSON 3.0

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in [RFC 2119](https://www.ietf.org/rfc/rfc2119.txt).

**Table of contents**

1. [Purpose](#1-purpose)
1. [File format](#2-file-format)
1. [Structure](#3-structure)
   1. [attribution](#31-attribution)
   1. [bounds](#32-bounds)
   1. [center](#33-center)
   1. [data](#34-data)
   1. [description](#35-description)
   1. [grids](#36-grids)
   1. [legend](#37-legend)
   1. [maxzoom](#38-maxzoom)
   1. [minzoom](#39-minzoom)
   1. [name](#310-name)
   1. [scheme](#311-scheme)
   1. [template](#312-template)
   1. [tilejson](#313-tilejson)
   1. [tiles](#314-tiles)
   1. [vector_layers](#315-vector_layers)
   1. [version](#316-version)
1. [Examples](#4-examples)

# 1. Purpose

This specification attempts to create a standard for representing metadata about multiple types of web-based map layers, to aid clients in configuration and browsing.

# 2. File Format

TileJSON manifest files use the JSON format as described in [RFC 4627](https://www.ietf.org/rfc/rfc4627.txt).

# 3. Structure

Implementations MUST treat unknown keys as if they weren't present. However, implementations MUST expose unknown key/values in their API so that API users can optionally handle these keys. Implementations MUST treat invalid values for keys as if they weren't present. If the key is required, implementations MUST treat the entire TileJSON manifest file as invalid and refuse operation.

## 3.1 `attribution`

REQUIRED|OPTIONAL (description of dependents/dependencies)

**Description:**

**Example:**

## 3.2 `bounds`

OPTIONAL. Default: [-180, -90, 180, 90].

The maximum extent of available map tiles. Bounds MUST define an area covered by all zoom levels. The bounds are represented in WGS:84 latitude and longitude values, in the order left, bottom, right, top. Values may be integers or floating point numbers. The minimum/maximum values for longitude and latitude are -180/180 and -90/90 respectively. Bounds MUST NOT "wrap" around the ante-meridian. If bounds are not present, the default value assumes the set of tiles is globally distributed.

```JSON
{
  "bounds": [ -180, -85.05112877980659, 180, 85.0511287798066 ]
}
```

## 3.3 `center`
## 3.4 `data`
## 3.5 `description`
## 3.6 `grids`
## 3.7 `legend`
## 3.8 `maxzoom`
## 3.9 `minzoom`
## 3.10 `name`
## 3.11 `scheme`
## 3.12 `template`
## 3.13 `tilejson`
## 3.14 `tiles`
## 3.15 `vector_layers`
## 3.16 `version`

# 4. Examples

Examples can be found in the example/ directory.
