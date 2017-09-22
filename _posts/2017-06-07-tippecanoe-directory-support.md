---
category: tech
tags: notes
title: Introducing Tippecanoe Directory Support
meta: This post describes how we have modified Tippecanoe to achieve directory support.
author: Joy Shan-Chun Kuo
---

In a [previous post](/tiler) James Milner introduced [Tiler](https://github.com/geovation/tiler), which utilizes tools such as [ogr2ogr](http://www.gdal.org/ogr2ogr.html) from GDAL and [Tippecanoe](https://github.com/mapbox/tippecanoe) from Mapbox to convert geospatial vector data into a directory of raw vector tiles for further use. Prior to my pull request, Tippecanoe was only able to generate and use MBtiles rather than directories of vector tiles. This post will introduce the process of modification for Tippecanoe to realize directory support in detail.

## What are the advantages of Vector Tiles?

Vector tiles can be rendered with JavaScript APIs like [Mapbox GL JS](https://www.mapbox.com/mapbox-gl-js/api/), [Leaflet.VectorGrid](https://leaflet.github.io/Leaflet.VectorGrid/vectorgrid-api-docs.html) or [OpenLayers](https://openlayers.org/en/latest/examples/osm-vector-tiles.html). Another advantage is that only the vector tiles requested via the browser will be fetched, as shown in Figure 1.

<figure>
<img src="/assets/vector-tiles-from-directory-network.png" alt="Image of vector tiles from directory network">
    <figcaption>Figure 1. Network activity of fetching vector tiles from directory</figcaption>
</figure>

## Why is directory structure support useful?

The [MBtile file format](https://www.mapbox.com/help/define-mbtiles/) stores millions of compressed tiles in a single SQLite database. Mapbox's own hosting services are able to understand this format and can use it to provide the specific vector tiles that a Mapbox GL JS client needs to load to display the current map view.

Mapbox GL JS is also able to fetch these tiles if they are hosted at a URL in this structure `/zoom/x/y.pbf` where `zoom` is the number representing the zoom level, and `x` and `y` are the coordinates in tile space.

If we could extract each `.pbf` file from the `.mbtiles` file and place it in the correct directory strucutre, it would be possible to host on a conventional static file hosting such as Amazon S3 or Google Cloud Storage (which has price advantages too). 

## Why is merging tiles useful?

Here we mainly talk about vector tiles, because raster tiles can also be merged. By merging vector tiles we can join features and their attributes together with more information for all the named layers. For example, if there are two directories that one contains vector tiles of all roads in the UK and the other contains vector tiles of all buildings in the UK, we can merge these two directories to get the vector tiles of all roads and buildings for the whole UK, and the new vector tiles will consist of a road layer and a building layer. Additionally, if there are more new road features, we can do the same action to merge them with current vector tiles to attain the expected result, and so on.

## Changes Made for Tippecanoe

Tippecanoe was designed to build vector tilesets from large collections of GeoJSON features and originally the only export format was MBtiles. We want to use Tippecanoe to get raw `.pbf` files in Tiler. For our use case, we had to run [mbutil](https://github.com/mapbox/mbutil) to export a `.mbtiles` file generated from Tippecanoe to a directory of PBF files and ungzip them to extract raw [protobuf data](https://developers.google.com/protocol-buffers/docs/overview). It seemed to be extra work because PBFs are always compressed and then put into the SQLite database during the Tippecanoe operation. There should be more options for users if they require a directory of PBF files. Therefore, I started the task to modify Tippecanoe to achieve our goal as follows:

* **No Tile Compression** - Add an option `-pC` / `--no-tile-compression` to avoid the compression of PBF vector tile data.
* **Output to Directory** - Add an option `-e` / `--output-to-directory` to write vector tiles to the specified directory instead of to an MBtiles file. I created a new function called [dir_write_tile()](https://github.com/mapbox/tippecanoe/blob/master/dirtiles.cpp#L17) in the Tippecanoe code to realize this action.  Besides vector tiles, there is also a `metadata.json` file produced in the directory, which contains the same information as the metadata table in the MBtiles file.

By combining the above options we can now generate a directory of raw PBF files with this command:

```
tippecanoe --no-feature-limit --no-tile-size-limit --no-tile-compression --output-to-directory NamedPlace NamedPlace.json
```

All the uncompressed vector tiles from layer `NamedPlace` will be stored into the specified directory as depicted in Figure 2.

<figure>
<img src="/assets/directory-of-pbfs.png" alt="Image of directory of pbfs" style="width:70%; display:block; margin:auto;">
    <figcaption>Figure 2. Directory of raw PBF vector tiles from layer NamedPlace</figcaption>
</figure>

## Input / Output Directories for Tile-join

After successfully implementing `--no-tile-compression` and `--output-to-directory`, we moved onto the next task in Tiler, which was to merge two directories of vector tiles with overlap. There is a tool in Tippecanoe called [tile-join](https://github.com/mapbox/tippecanoe#tile-join) that is able to combine contents from multiple source MBtiles files into a new MBtiles output. The goal here is to modify Tippecanoe's `tile-join` to merge multiple source directories of vector tiles and export them to a new directory of raw PBF files rather than an MBtiles file. We also built a [merge function](https://github.com/Geovation/tiler/blob/master/tiler/tiler-scripts/merge.py) in Tiler to achieve the same goal.

The main problem for `tile-join` when inputting from a directory is to ensure the order of vector tiles is compatible with the MBtiles format. Figure 3 shows the vector tiles in `NamedPlace.mbtiles` using [tippecanoe-enumerate](https://github.com/mapbox/tippecanoe#tippecanoe-enumerate). Each line of the output lists the name of MBtiles and the `zoom`, `x`, and `y` coordinates of vector tile. The order of this list corresponds to the order in which these assets will be selected from the SQLite database.

<figure>
<img src="/assets/tippecanoe-enumerate.png" alt="Image of tippecanoe-enumerate" style="width:60%; display:block; margin:auto;">
    <figcaption>Figure 3. Partial screenshot of vector tiles record in NamedPlace.mbtiles</figcaption>
</figure>

It is noticeable that the `y` index of tiles in Figure 3 is in descending order under the same `zoom` and `x` coordinate. This order must be maintained when implementing directory traversal. To guarantee this order I utilized a C library function [scandir()](http://man7.org/linux/man-pages/man3/scandir.3.html) in the header file `dirent.h` to create an extra function called [read_dir()](https://github.com/mapbox/tippecanoe/blob/master/tile-join.cpp#L344) in the `tile-join` code to recursively walk through the specified directory and its subdirectories to parse the path of all PBF files in the right order for MBtiles.

However, another problem emerged during the testing of `read_dir()`. That is, the order of input directory might differ based on the operating system used. As can be seen in Figure 2, the directory listing is in lexicographic order MacOS but in fact what we require is numerical order. As a matter of experience, the fastest solution is to use `versionsort()` function as the forth argument in `scandir()`. Unfortunately, it is only supported in GNU OS, so as an alternative  I used `alphasort()` function and a new integer variable `zoom_range` to handle the input in numerical order.

After solving these two tricky problems I thought there would be no more obstacles ahead, but then a new issue arose. It seemed that the `d_type` field in [dirent structure](http://man7.org/linux/man-pages/man3/readdir.3.html) was only fully supported by some file systems such as `Btrfs`, `ext2`, `ext3`, and `ext4` but not others, so it might return `DT_UNKNOWN` as undetermined type for a directory rather than `DT_DIR`. As a result, I used [lstat(2)](https://linux.die.net/man/2/lstat) system call in `read_dir()` function to avoid this potential risk.

After all the various adjustment for `read_dir()`, I created another new function called [dir_read_tile()](https://github.com/mapbox/tippecanoe/blob/master/dirtiles.cpp#L8) in the Tippecanoe code to read PBF data from a given path. In addition, the `metadata.json` file in the input directory has to be analysed and regenerated later in the decoding stage of `tile-join`. Once this was implemented the `tile-join` input directory functionality was achieved. With the help of pointers, multiple input directories could be joined in a single command. The `--no-tile-compression` and `--output-to-directory` options I implemented earlier can be used to enrich the options for `tile-join`.

As a result, `tile-join` can now use a directory of tiles as source, and can output a directory of `.pbf` files instead of a `.mbtiles` file. Multiple source MBtiles files and directories of tiles are feasible as well. In addition, there is also a new option `-pC` for no tile compression which is the same as in Tippecanoe. For example, we can merge two directories of tiles (`Road` and `NamedPlace`) and generate a directory of raw PBF files with this command:

```
tile-join -pk -pC -e Road_NamedPlace_Merged Road NamedPlace
```

Any combination of input MBtiles and directories is supported, and CSV files can also be used as the source for new attributes to join to the features. If we require an MBtiles output by merging `Road.mbtiles` and a directory of tiles `NamedPlace`, it is achievable with this command:

```
tile-join -pk -f -o merged.mbtiles Road.mbtiles NamedPlace
```

In order to test whether the new version of tile-join is correct, we can use [tippecanoe-decode](https://github.com/mapbox/tippecanoe#tippecanoe-decode) to convert the vector MBtiles output to GeoJSON for comparison with this command:

```
tippecanoe-decode merged.mbtiles > merged.mbtiles.json.check
```

The first twenty lines of `merged.mbtiles.json.check` are shown below:

```
{ "type": "FeatureCollection", "properties": {
"bounds": "-0.615234,50.331436,1.494141,51.699800",
"center": "0.384521,51.351200,14",
"description": "NamedPlace",
"format": "pbf",
"json": "{\"vector_layers\": [ { \"id\": \"NamedPlace\", \"description\": \"\", \"minzoom\": 0, \"maxzoom\": 14, \"fields\": {\"CLASSIFICA\": \"String\", \"DISTNAME\": \"String\", \"FEATCODE\": \"Number\", \"FONTHEIGHT\": \"String\", \"HTMLNAME\": \"String\", \"ID\": \"String\", \"ORIENTATIO\": \"Number\"} }, { \"id\": \"Road\", \"description\": \"\", \"minzoom\": 0, \"maxzoom\": 14, \"fields\": {\"CLASSIFICA\": \"String\", \"DISTNAME\": \"String\", \"DRAWLEVEL\": \"String\", \"FEATCODE\": \"Number\", \"ID\": \"String\", \"OVERRIDE\": \"String\", \"ROADNUMBER\": \"String\"} } ] }",
"maxzoom": "14",
"minzoom": "0",
"name": "Road.mbtiles + NamedPlace",
"type": "overlay",
"version": "2"
}, "features": [
{ "type": "FeatureCollection", "properties": { "zoom": 0, "x": 0, "y": 0 }, "features": [
{ "type": "FeatureCollection", "properties": { "layer": "Road", "version": 2, "extent": 4096 }, "features": [
{ "type": "Feature", "properties": { "ID": "609031FB-A9DC-4D14-9FE0-688B97D2A52A", "DISTNAME": "Stony Lane", "CLASSIFICA": "Minor Road", "DRAWLEVEL": "0", "OVERRIDE": "F", "FEATCODE": 15750 }, "geometry": { "type": "LineString", "coordinates": [ [ -0.615234, 51.727028 ], [ -0.615234, 51.672555 ] ] } }
,
{ "type": "Feature", "properties": { "ID": "8D4D0186-E7EA-42AB-AC1E-069AC96E8452", "CLASSIFICA": "Restricted Local Access Road", "DRAWLEVEL": "0", "OVERRIDE": "F", "FEATCODE": 15762 }, "geometry": { "type": "LineString", "coordinates": [ [ -0.615234, 51.672555 ], [ -0.615234, 51.727028 ] ] } }
,
{ "type": "Feature", "properties": { "ID": "129ACDEF-E257-473C-A126-2A3DC6468038", "CLASSIFICA": "Minor Road", "DRAWLEVEL": "0", "OVERRIDE": "F", "FEATCODE": 15750 }, "geometry": { "type": "LineString", "coordinates": [ [ -0.615234, 51.672555 ], [ -0.615234, 51.727028 ] ] } }
,
```

The datasets for testing in this post [contain OS data © Crown copyright and database
right 2017](https://www.ordnancesurvey.co.uk/business-and-government/licensing/using-creating-data-with-os-products/os-opendata.html)

## Conclusion

Tippecanoe directory support has been implemented with success, including the `tile-join` tool. It can be beneficial to adopt a directory structure and merging process for handling vector tiles, which can be further used for rendering, as in [Tiler](/tiler/) and [building your own static vector tile pipeline](/build-your-own-static-vector-tile-pipeline/).

It was such a positive experience to make contributions to Mapbox's Tippecanoe. We hope that this achievement can provide a method for anyone in the need of vector tiles.

Feel free to leave a comment if you have any question.

[Creative Commons Share-a-like](https://creativecommons.org/licenses/by-sa/2.5/) Joy Shan-Chun Kuo &copy; [Geovation](http://geovation.uk/)
