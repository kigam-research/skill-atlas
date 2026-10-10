# Natural Earth land outline

- Dataset: Natural Earth 1:110m land, release v5.1.2.
- Source: https://github.com/nvkelso/natural-earth-vector/blob/v5.1.2/geojson/ne_110m_land.geojson
- License: public domain; https://github.com/nvkelso/natural-earth-vector/blob/v5.1.2/LICENSE.md
- Retrieved: 2026-10-10 through GitHub contents API (no tile service).
- Source SHA-256: 9e0729ee253ca7d7a5c4ae9395fb1902264c5377c52e224d13dd85010e2835d9
- Transformation: retain Polygon/MultiPolygon rings; Douglas-Peucker simplification at 0.18 degrees,
  omit degenerate rings (<4 points), round coordinates to 0.01 degrees; JSON is an array of lon/lat rings.
- Output: `natural-earth-land-110m.json`, 48,094 bytes. Context only, not a scientific regional boundary.
