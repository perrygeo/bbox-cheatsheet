# Reference Guide to Bounding Box implementations in geospatial software

The reference guide will allow you to quickly compare bounding boxes representation between software implementations and hopefully reduce a common source of bugs.

See the [README](https://github.com/perrygeo/bbox-cheatsheet/README.md) for a detailed discussion of the rationale.

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

Each software implementation listed here must have:

1. Name of software
2. Vocabulary
3. Example in normalized notation


-----------------------------------------------------------

software |      terminology|    example| notes
---------|-----------------|------------------|-------
postgis  |        extent   | `BOX(MINX MINY, MAXX MAXY)`
gdalinfo output|corner coordinates|`Lower Left ( west, south) ... Upper Right (north)`
gdal_translate input|projwin ulx uly lrx lry|`-projwin west north east south`
gdalwarp input|georeferenced extents, xmin ymin xmax ymax|`-te west south east north`
ogrinfo output|Extent|`Extent: (west, south) - (east, north)`
ogr2ogr/ogrinfo input|-spat xmin ymin xmax ymax|`-spat
GeoTools|Envelope/BoundingBox|
mapnik|extent/envelope|Box2d(minx,miny,maxx,maxy)
qgis|
arcgis|
elasticsearch|
fiona|
shapely|
GRASS|
saga|
GEOS|
JTS|


## Contributing

I'm looking for some help to make this a complete reference for the GIS software community. If you have any additions, corrections or suggestions for how to organize and visualize this information, file an issue or a pull request on github.


