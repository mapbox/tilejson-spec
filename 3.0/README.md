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

OPTIONAL. Default: null.

Contains an attribution to be displayed when the map is shown to a user. Implementations MAY decide to treat this as HTML or literal text. For security reasons, make absolutely sure that this field can't be abused as a vector for XSS or beacon tracking.

```JSON
{
  "attribution": "<a href='http://openstreetmap.org'>OSM contributors</a>"
}
```

## 3.2 `bounds`
## 3.3 `center`
## 3.4 `data`
## 3.5 `description`

OPTIONAL. Default: null.

A text description of the tileset. The description can contain any legal character. Implementations SHOULD NOT interpret the description as HTML.

```JSON
{
  "description": "We are volcanoes. When we women offer our experience as our truth, as human truth, all the maps change. There are new mountains."
}
```

## 3.6 `grids`
## 3.7 `legend`
## 3.8 `maxzoom`
## 3.9 `minzoom`

## 3.10 `name`

OPTIONAL. Default: null.

A name describing the tileset. The name can contain any legal character. Implementations SHOULD NOT interpret the name as HTML.

```JSON
{
  "name": "Earthsea v2"
}
```

## 3.11 `scheme`
## 3.12 `template`
## 3.13 `tilejson`

REQUIRED.

A semver.org style version number. Describes the version of the TileJSON spec that is implemented by this JSON object.

```JSON
{
  "tilejson": "3.0"
}
```

## 3.14 `tiles`
## 3.15 `vector_layers`
## 3.16 `version`

OPTIONAL. Default: "1.0.0".

A [semver.org](https://semver.org) style version number representing the vector tiles themselves. When changes across tiles are introduced, such as polygon or linestring geometry updates, the minor version MUST change. This may lead to cut off labels. Therefore, implementors can decide to clean their cache when the minor version changes. Changes to the patch level MUST only have changes to tiles that are contained within one tile. When tiles change significantly, the major version MUST be increased. Implementations MUST NOT use tiles with different major versions.

```JSON
{
  "version": "1.0.0"
}
```

# 4. Examples

Examples can be found in the example/ directory.
