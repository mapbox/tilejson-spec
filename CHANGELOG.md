## 3.0.0 (Aug 18th, 2021)

- **BREAKING**: document and require `vector_layers` key for all vector tilesets
- Further clarify existing fields and update format of the specification
- Include notes and comments about overzooming when discussing maxzoom and fillzoom
- Consistify usage of "keys" or "fields" across the spec
- Consistify usage of "set of tiles"
- Clearly define the word "implementation"
- Add note about "interactivity" regarding UTF-Grids and how this field predates GL-based maps
- Clarify if `bounds` values can be the same to represent a single point tileset
- Remove wiki and add implementations to README

Thanks reviewers! @pnorman, @stevage, @samanpwbb, @asheemmamoowala

## 2.2.0 (Feb 3rd, 2017)

- Increase suggested possible `maxzoom` value from `22` to `30`

## 2.1.0 (June 19th, 2012)

- `data` key added, [pull request](https://github.com/mapbox/tilejson-spec/commit/f576ee2491f888a14d1acb68a0704d160bf4747b)
- removed experimental `resolution` parameter

## 2.0.1 (May 29th, 2012)

- Add experimental `resolution` parameter (no longer supported), [commit](https://github.com/mapbox/tilejson-spec/commit/f576ee2491f888a14d1acb68a0704d160bf4747b)

## 2.0.0 (Nov 2nd, 2011)

- `formatter` renamed to `template` to work with mustache templates, per changes to the [utfgrid-spec version 1.2](https://github.com/mapbox/utfgrid-spec/blob/master/CHANGELOG.md#12)

## 1.0.0 (July 13th, 2011)

- first 🎉
