---
layout: post
title: Introducing Tiler: a Config Orientied Vector Tile Pipeline
meta: 
author: James Milner
comments: false
---

# Introducing Tiler

One key thing we  are always interested in exploring at the Geovation Hub is the future of mapping an geospatial technologies. For a long time mapping has relied on stitching together images at different zoom levels to produce the traditional web map.

In recent times, there has been a push towards using the underlying vector data (i.e. Points, Lines, Polygons) that produce those tiles rather than an image (raster) representation of that data. This has been led predominantly by Mapbox underpinned by their open [Vector Tile specification](https://github.com/mapbox/vector-tile-spec). The specification has gained traction with other mapping providers such as Esri and Mapzen.

Vector Tiles allow for a host of benefits including restyling on the client, generally less pixelation, smoother transitions between zoom layers, and hardware acceleration from the GPU using HTML5 Canvas or WebGL. In addition things like labels can also be dynamic and retain orientation with rotation of the map.

One thing we found when exploring how we might take Ordnance Survey Open Data such as [Open Map Local](https://www.ordnancesurvey.co.uk/opendatadownload/products.html#OPMPLC) and turn them into vector tiles is a lot of the pipelines and processes for creating the raw underlying tiles were some what fragmented, convoluted or restricting.

Thankfully we found lots of tools that did individual pieces of what we needed. We just needed to package them together. This was when the idea of [Tiler](https://github.com/geovation/tiler) was born. Tiler aims to be a (relatively) simple to use pipeline for taking traditional vector data from sources like Shapefiles, GeoJSON, GML or PostGIS and turning them into raw Vector Tiles that you can use with clients such as Mapbox GL or OpenLayers (as examples).

Tiler bundles together some powerful tools, abstracting them away into a configuration oriented approach to generating tiles. One defines a JSON config file for a series of data and then can produce tiles using that config with Tiler.

## A Worked Example

Let's say we took the TQ tile from the Ordnance Survey Open Map Local Dataset and we wanted to make some Vector Tiles for the woodlands layer of the data set. Firstly we convert the data to WGS84 with the [OSTN02](https://www.ordnancesurvey.co.uk/business-and-government/help-and-support/navigation-technology/os-net/ostn02-ntv2-format.html) transformation file, using tilers bng2wgs84 python script. We can do this by using `./run.sh --shell` and then `python bng2wgs84.py some_directory` where some directory is the location of your woodlands dataset (the shapefile). This will create a new file, for example `TQ_Building_WGS84.shp`.

We could generate that into a set of tiles using a config such as the one shown:


```javascript
{
    "outdir" : "/tiler-data/tiles/",
    "tileset" : "TQ_Buildings",
    "simplification" : 1,
    "data" : {
        "buildings" : {
            "type" : "shapefile",
            "paths" : ["/tiler-data/input/TQ_Building_WGS84.shp"],
            "minzoom" : 1,
            "maxzoom" : 12
        }
    }

}
```

We save this to the `configs` folder of Tiler as `woodland.tiler.json`. When we run tiler from the host (`./run.sh woodland`) or within the docker shell (`tiler woodland`) it will produce a set of vector tiles in the `/tiler-data/tiles/` directory of tiler when we do `./run.sh buildings` from the tiler root directory.

We end up with a set of tiles from zoom level 1-12:

![Image of the file tree for the tiles](/assets/tiles.png)

And we can view those in MapboxGL with a bit of style tinkering:

![Image of woodlands being show in MapboxGL](/assets/woodland.png)

## Consistent Environments

Tiler uses Docker; a platform for sharing applications via containers which bundle up complex applications into 'images'. This allows us to combine tools such as ogr2ogr (for geographic data format translation) and Tippecanoe (for GeoJSON to vector tile translation) and harness there functionality into a streamline pipeline that works across platforms (Windows, Mac, Linux etc).