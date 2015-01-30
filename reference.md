# Reference Guide to Bounding Box implementations in geospatial software

The reference guide will allow you to quickly compare bounding boxes representation between software implementations and hopefully reduce a common source of bugs.

See the [README](https://github.com/perrygeo/bbox-cheatsheet/blob/master/README.md) for a detailed discussion of the rationale and read the rules below.

## Bounding Box Cheatsheet

software | terminology     |  example representation   |   normalized representation
---------|-----------------|---------------------------|-----------------------------
postgis  |        extent   ||  `BOX(west south, east north)`
gdalinfo output|corner coordinates||`Lower Left ( west, south )` `Upper Right ( east, north )`
gdal_translate input|Subwindow, geographic coordinates|`-projwin ulx uly lrx lry`|`-projwin west north east south`
gdalwarp input|georeferenced extents|`-te xmin ymin xmax ymax`|`-te west south east north`
ogrinfo output|Extent||`Extent: (west, south) - (east, north)`
ogr2ogr and ogrinfo input||`-spat xmin ymin xmax ymax`|`-spat west south east north`
mapnik|extent, envelope|`Box2d(minx,miny,maxx,maxy)`|`Box2d(west,south,east,north)`
qgis (dekstop interface)|Extent||`west,south : east,north`
shapely|bounding box,bounds|`(minx, miny, maxx, maxy)`|`(west, south, east, north)`
fiona|bounding box,bounds,mbr|`(minx, miny, maxx, maxy)`|`(west, south, east, north)`
rio (rasterio cli)|bounds|GeoJSON FeatureCollection|
grass|region, extent, bounding box|`n, s, w, e`|`north, south, west, east`
saga|extent, bbox|`xMin, yMin, xMax, yMax`|`west, south, east, north`



## Contributing

The rules are simple though somewhat arbitrary. But for the sake of normalizing
the bounding box representation, we'll use the following notation:

1. `west`: Lower-left X coordinate
1. `south`: Lower-left Y coordinate
1. `east`: Upper-right X coordinate
1. `north`: Upper-right Y coordinate

So while the software docs may list a bounding box as

```
(minx, miny, maxx, maxy)
```

the *normalized notation* would be

```
(west, south, east, north)
```

Each software implementation listed here should have:

1. Name of software
2. Vocabulary
3. Example in the "native" notation
3. Example in the normalized notation (west, south, east, north)


I'm looking for some help to make this a complete reference for the GIS software community. If you have any additions, corrections or suggestions for how to organize and visualize this information, file an issue or a pull request on github.


