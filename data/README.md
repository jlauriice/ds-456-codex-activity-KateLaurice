# Minneapolis off-sale liquor records

Downloaded November 11, 2026, from the City of Minneapolis public ArcGIS service.

- [Dataset page](https://opendata.minneapolismn.gov/datasets/cd28878d04ce41e6b09f37ea5d62bef0_0/explore)
- [Layer and field metadata](https://services.arcgis.com/afSMGVsC7QlRK1kZ/arcgis/rest/services/Off_Sale_Liquor/FeatureServer/0?f=pjson)
- [ArcGIS item description](https://www.arcgis.com/sharing/rest/content/items/cd28878d04ce41e6b09f37ea5d62bef0?f=pjson)

`off_sale_liquor.json` is the unchanged JSON query response, including attributes
and point geometry. Query parameters: `where=1=1`, `outFields=*`, `outSR=4326`,
`f=json`, `orderByFields=OBJECTID`, `resultRecordCount=2000`.
`off_sale_liquor_metadata.json` contains the source field definitions.
`off_sale_liquor_count.json` is the separate `returnCountOnly=true` query response:
180 records. The notebook checks this count against the downloaded feature count
and rejects a response marked as truncated.

The original service uses Web Mercator (EPSG:3857 / ArcGIS WKID 102100).
The downloaded geometry was requested in EPSG:4326 (longitude/latitude degrees);
the original coordinate attributes remain unchanged.
ArcGIS date attributes are milliseconds since the Unix epoch, with UTC time
reference in the layer metadata. The notebook converts them to UTC timestamps.

The city item describes monthly refreshes on the last Friday at 7:30 PM and
possible delays in downloadable exports. The item supplies a CC0 dedication.
This is a saved snapshot, not a continuously refreshed inventory.

The exploration retains all records. It flags seven points at `(0, 0)` and
excludes them only from the location plot. Repeated license numbers are displayed
for inspection; they are not silently deduplicated. Counts of records, license
numbers, and addresses should not be treated as interchangeable business counts.

Analysis and outputs: [notebook](../analysis/codex_activity.ipynb),
[status chart](../analysis/figures/license_status.png),
[location plot](../analysis/figures/license_locations.png).

## Neighborhood boundaries for mapping

Downloaded November 11, 2026, from the City's
[Minneapolis Neighborhoods layer](https://services.arcgis.com/afSMGVsC7QlRK1kZ/ArcGIS/rest/services/Minneapolis_Neighborhoods/FeatureServer/0).

`minneapolis_neighborhoods.geojson` is the unchanged query response with
`where=1=1`, `outFields=*`, `outSR=4326`, `f=geojson`, and
`resultRecordCount=2000`. The separate count response in
`minneapolis_neighborhoods_count.json` confirms 87 polygons.
`minneapolis_neighborhoods_metadata.json` preserves the source metadata.
The notebook checks the feature count and coordinate reference system.

The polygons provide context; no neighborhood-name join is performed. Both the
license geometry and boundary vertices are converted from EPSG:4326 to EPSG:3857
for the map. The map uses one point per distinct license number after checking
that repeated IDs have identical attributes apart from OBJECTID. It plots 151
licenses, including 51 Approved licenses. The seven licenses at zero coordinates
are saved in [the review CSV](../analysis/unmapped_licenses.csv).

[Map with neighborhood boundaries](../analysis/figures/liquor_license_map.png).
