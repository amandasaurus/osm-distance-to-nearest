# `osm-distance-to-nearest`

> “You're in a gun shop. How far away is the nearst bank?”

This script processes OpenStreetMap data to answer questions like that. 

You give it an OSM data file, and for everything tagged one way (e.g. [`shop=weapons`](https://wiki.openstreetmap.org/wiki/Tag:shop%3Dweapons) for gun shops), it finds the closest thing with the other tag(s) ([`amenity=bank`](https://wiki.openstreetmap.org/wiki/Tag:amenity%3Dbank) for banks, [`amenity=atm`](https://wiki.openstreetmap.org/wiki/Tag:amenity%3Datm) for ATMs).

## Usage

```bash
osm-distance-to-nearest -i DATA.osm.pbf -a shop=weapons -b amenity=atm,bank -o guns_banks_
```

Creates `guns_banks_distances.csv` (and `guns_banks_distances.geojson`)

* `-i FILENAME.osm.pbf`:  Input filename to process. Takes anything `osmium` can read.
* `-a OSMIUM_FILTER`: The [`osmium tags-filter` filter expression](https://osmcode.org/osmium-tool/manual.html#filtering-by-tags) to select the A objects
* `-b OSMIUM_FILTER`: tags filter to select B
* `-o OUTPUT_PREFIX`: *(optional)* text to prefix for files

## CSV output

There is one row for every OSM element tagged with the `-a` filter, and it has
been matched up to the row with `-b` which is the closest.

* `a_tags`: OSM tags for object A. Encoded in JSON, and escaped for CSV
* `a_osm_type`: Text showing OSM Type for A: either `node`, `way` or `relation`
* `a_osm_id`: Integer of the OSM ID for A
* `b_tags`: OSM tags for object B. Encoded in JSON, and escaped for CSV
* `b_osm_type`: Text showing OSM Type for B: either `node`, `way` or `relation`
* `b_osm_id`: Integer of the OSM ID for B
* `distance_m`: Distance in metres between A & B. Uses great circle calculation. “As the crow flies distance”
* `a_point_wkt`: Centroid of object A, encoded as a [Well Known Text](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry) point
* `a_point_geojson`: Centroid of A, as a GeoJSON geometry
* `a_point_lat`: Latitide of the centroid of A. Floating point number
* `a_point_lng`: Longitude of the centroid of A. Floating point number
* `b_point_wkt`: Centroid of object B, encoded as a [Well Known Text](https://en.wikipedia.org/wiki/Well-known_text_representation_of_geometry) point
* `b_point_geojson`: Centroid of B, as a GeoJSON geometry
* `b_point_lat`: Latitide of the centroid of B. Floating point number
* `b_point_lng`: Longitude of the centroid of B. Floating point number
* `line_wkt`: Line from A to B, encoded as WKT
* `line_geojson`: Line from A to B, encoded as GeoJSON geometry

## See Also

* [`osmium`](https://osmcode.org/osmium-tool/): Tags are filtered with osmium tool.

## Inspiration

[Vincent Privat](https://wiki.openstreetmap.org/wiki/User:Don-vip) posted on [twitter](https://twitter.com/VincentPrivat/status/1567689683022319616) of a gun shop across the road from a bank (technially an ATM). [WeeklyOSM](https://weeklyosm.eu) reported in [№634](https://weeklyosm.eu/archives/15945). So it got me thinking.

## Copyright

Copyright, GNU Affero General Public Licence v3 (or later) © 2022 Amanda McCann <amanda@technomancy.org>. See [LICENCE](./LICENCE)

Data & Derived Work from the OpenStreetMap project is open data, licensed under the [Open Data Commons Open Database License (ODbL)](https://opendatacommons.org/licenses/odbl/), and [© OpenStreetMap Contributors](https://www.openstreetmap.org/copyright)
