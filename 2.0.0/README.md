# TileJSON 2.0.0

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
"SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in
this document are to be interpreted as described in RFC 2119.

## 1. Purpose

This specification attempts to create a standard for representing
metadata about multiple types of web-based layers, to aid clients
in configuration and browsing.

## 2. File format

TileJSON manifest files use the JSON format as described in RFC 4627.

Implementations MUST treat unknown keys as if they weren't present.
However, implementations MUST expose unknown key/values in their API
so that API users can optionally handle these keys. Implementations MUST
treat invalid values for keys as if they weren't present. If the key is
required, implementations MUST treat the entire TileJSON manifest file
as invalid and refuse operation.


```javascript
{
    // REQUIRED. A semver.org style version number. Describes the version of
    // the TileJSON spec that is implemented by this JSON object.
    "tilejson": "2.0.0",

    // OPTIONAL. Default: null. A name describing the tileset. The name can
    // contain any legal character. Implementations SHOULD NOT interpret the
    // name as HTML.
    "name": "compositing",

    // OPTIONAL. Default: null. A text description of the tileset. The
    // description can contain any legal character. Implementations SHOULD NOT
    // interpret the description as HTML.
    "description": "A simple, light grey world.",

    // OPTIONAL. Default: "1.0.0". A semver.org style version number. When
    // changes across tiles are introduced, the minor version MUST change.
    // This may lead to cut off labels. Therefore, implementors can decide to
    // clean their cache when the minor version changes. Changes to the patch
    // level MUST only have changes to tiles that are contained within one tile.
    // When tiles change significantly, the major version MUST be increased.
    // Implementations MUST NOT use tiles with different major versions.
    "version": "1.0.0",

    // OPTIONAL. Default: null. Contains an attribution to be displayed
    // when the map is shown to a user. Implementations MAY decide to treat this
    // as HTML or literal text. For security reasons, make absolutely sure that
    // this field can't be abused as a vector for XSS or beacon tracking.
    "attribution": "<a href='http://openstreetmap.org'>OSM contributors</a>",

    // OPTIONAL. Default: null. Contains a mustache template to be used to
    // format data from grids for interaction.
    // See https://github.com/mapbox/utfgrid-spec/tree/master/1.2
    // for the interactivity specification.
    "template": "{{#__teaser__}}{{NAME}}{{/__teaser__}}",

    // OPTIONAL. Default: null. Contains a legend to be displayed with the map.
    // Implementations MAY decide to treat this as HTML or literal text.
    // For security reasons, make absolutely sure that this field can't be
    // abused as a vector for XSS or beacon tracking.
    "legend": "Dangerous zones are red, safe zones are green",

    // OPTIONAL. Default: "xyz". Either "xyz" or "tms". Influences the y
    // direction of the tile coordinates.
    // The global-mercator (aka Spherical Mercator) profile is assumed.
    "scheme": "xyz",

    // REQUIRED. An array of tile endpoints. {z}, {x} and {y} are replaced with
    // the corresponding integers. If multiple endpoints are specified, clients
    // may use any combination of endpoints. All endpoints MUST return the same
    // content for the same URL. The array MUST contain at least one endpoint.
    "tiles": [
        "http://localhost:8888/admin/1.0.0/world-light,broadband/{z}/{x}/{y}.png"
    ],

    // OPTIONAL. Default: []. An array of interactivity endpoints. {z}, {x}
    // and {y} are replaced with the corresponding integers. If multiple
    // endpoints are specified, clients may use any combination of endpoints.
    // All endpoints MUST return the same content for the same URL.
    // If the array doesn't contain any entries, interactivity is not supported
    // for this tileset.
    // See https://github.com/mapbox/mbtiles-spec/blob/master/1.1/interaction.md
    // for the interactivity specification.
    "grids": [
        "http://localhost:8888/admin/1.0.0/broadband/{z}/{x}/{y}.grid.json"
    ],

    // OPTIONAL. Default: 0. >= 0, <= 22.
    // An integer specifying the minimum zoom level.
    "minzoom": 0,

    // OPTIONAL. Default: 22. >= 0, <= 22.
    // An integer specifying the maximum zoom level. MUST be >= minzoom.
    "maxzoom": 11,

    // OPTIONAL. Default: [-180, -90, 180, 90].
    // The maximum extent of available map tiles. Bounds MUST define an area
    // covered by all zoom levels. The bounds are represented in WGS:84
    // latitude and longitude values, in the order left, bottom, right, top.
    // Values may be integers or floating point numbers.
    "bounds": [ -180, -85.05112877980659, 180, 85.0511287798066 ],

    // OPTIONAL. Default: null.
    // The first value is the longitude, the second is latitude (both in
    // WGS:84 values), the third value is the zoom level as an integer.
    // Longitude and latitude MUST be within the specified bounds.
    // The zoom level MUST be between minzoom and maxzoom.
    // Implementations can use this value to set the default location. If the
    // value is null, implementations may use their own algorithm for
    // determining a default location.
    "center": [ -76.275329586789, 39.153492567373, 8 ]
}
```


## 3. Caching

Clients MAY cache files retrieved from a remote server.
When implementations decide to perform caching, they MUST honor valid
cache control HTTP headers as defined in the HTTP specification for both
tile images and the TileJSON manifest file.
