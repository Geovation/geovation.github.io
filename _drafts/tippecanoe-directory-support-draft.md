---
layout: post
title: Introducing Tippecanoe Directory Support
meta: This post describes how we have modified Tippecanoe to achieve directory support so as to utilize such function in Tiler and also other uses.
author: Joy Shan-Chun Kuo
comments: true
---

In the previous [James Milner's article](/2017/05/14/tiler/), he introduced the idea and usage of [Tiler](https://github.com/geovation/tiler), which utilizes tools such as [ogr2ogr](http://www.gdal.org/ogr2ogr.html) from GDAL and [Tippecanoe](https://github.com/mapbox/tippecanoe) from Mapbox so as to turn geospatial vector data format into a directory of raw vector tiles for further use. It is worth mentioning that tippecanoe at first did not have any function of handling directory of tiles but only MBtiles until the recent update from my pull requests. So this post will introduce the process of modification for tippecanoe to realize directory support in detail.

## Why directory structure support is useful?

The purpose of using directory structure is to make the hosting more easily. As definition, [MBtiles](https://www.mapbox.com/help/an-open-platform/#mbtiles) is a file format for storing millions of tiles in a single SQLite database, which is not supported in Amazon S3 or Google Cloud Storage but only hosting. If the format of tiles record in MBtiles is `pbf`, then these tiles are corresponding to vector tiles. Thus, it is more convenient to use directory of vector tiles instead of MBtiles in general case. Besides, if we already have a directory of tiles, it is not necessary to convert them to MBtiles in order to tweak the content.

Directory of vector tiles can be rendered with JavaScript APIs like [Mapbox GL JS](https://www.mapbox.com/mapbox-gl-js/api/), [Leaflet.VectorGrid](https://leaflet.github.io/Leaflet.VectorGrid/vectorgrid-api-docs.html) or [OpenLayers](https://openlayers.org/en/latest/examples/osm-vector-tiles.html). Another advantage of using directory is that only the vector tiles requested via the browser will be fetched, as shown in Figure 1.

<figure>
<img src="/assets/vector-tiles-from-directory-network.png" alt="Image of vector tiles from directory network">
    <figcaption>Figure 1. Network activity of fetching vector tiles from directory</figcaption>
</figure>

## Why merging tiles is useful?

Here we mainly talk about vector tiles, because raster tiles can also be merged. By merging vector tiles, we can join features and their attributes together with more information for all the named layers. For example, if there are two directories that one contains vector tiles of all roads in the UK and the other contains vector tiles of all buildings in the UK, we can merge these two directories to get the vector tiles of all roads and buildings for the whole UK, and the new vector tiles will consist of a road layer and a building layer. Additionally, if there are more new road features, we can do the same action to merge them with current vector tiles to attain the expected result, and so on.

## Changes Made for Tippecanoe

Tippecanoe was designed to build vector tilesets from large collections of GeoJSON features, and originally the export option was only MBtiles, while we aimed to use tippecanoe to get raw `.pbf` files in Tier. For our use case, we had to run [mbutil](https://github.com/mapbox/mbutil) to export a `.mbtiles` file generated from tippecanoe to a directory of PBF files and ungzip them to extract raw [protobuf data](https://developers.google.com/protocol-buffers/docs/overview). It seemed to be extra work because apparently PBFs are always compressed and then put into the SQLite database during the tippecanoe operation. Therefore, I started the task to modify tippecanoe to achieve our goal.

* No Tile Compression

Add an option `-pC` / `--no-tile-compression` to avoid the compression of PBF vector tile data.

* Output to Directory

Add an option `-e` / `--output-to-directory` to write vector tiles to the specified directory instead of to an MBtiles file. I created a new function called [dir_write_tile()](https://github.com/mapbox/tippecanoe/blob/master/dirtiles.cpp#L17) in the tippecanoe code to realize this action. Besides, there is also a `metadata.json` file produced in the directory, which contains important information about such layer.

By the combination of the above two options, take `NamedPlace.json` as an example, we can finally generate a directory of raw PBF files with this command:

```
tippecanoe --no-feature-limit --no-tile-size-limit --no-tile-compression --output-to-directory NamedPlace NamedPlace.json
```

All the uncompressed vector tiles from layer `NamedPlace` will be stored into the specified directory as depicted in Figure 2.

<figure>
<img src="/assets/directory-of-pbfs.png" alt="Image of directory of pbfs" style="width:70%; display:block; margin:auto;">
    <figcaption>Figure 2. Directory of raw PBF vector tiles from layer NamedPlace</figcaption>
</figure>

* Input / Output Directories for Tile-join

After successfully implementing the function of no tile compression and output to directory, we moved onto the next task in Tiler, which was to merge two directories of vector tiles with overlap. At the same time, we found that there is a tool in tippecanoe called tile-join, and it is able to combine contents from multiple source MBtiles files to a new MBtiles output. Here again we encountered the similar issue as the previous one but way more complicated, so the new goal was to modify tippecanoe's tile-join to merge multiple source directories of vector tiles and export to a new directory of raw PBF files rather than an MBtiles file.

The main problem for input from directory is to make sure the order of vector tiles is identical with MBtiles. Let's first take a look at Figure 3, it shows the vector tiles in `NamedPlace.mbtiles` by using [tippecanoe-enumerate](https://github.com/mapbox/tippecanoe#tippecanoe-enumerate). Each line of the output lists the name of MBtiles and the zoom, x, and y coordinates of vector tile corresponding to the order while selecting it from SQLite database.

<figure>
<img src="/assets/tippecanoe-enumerate.png" alt="Image of tippecanoe-enumerate" style="width:60%; display:block; margin:auto;">
    <figcaption>Figure 3. Partial screenshot of vector tiles record in NamedPlace.mbtiles</figcaption>
</figure>

It is noticeable that the y index of tiles in Fig. 3 is in descending order under the same zoom and x coordinate, so this is a significant part to attain during implementation of directory traversing. To gurantee the process without conflict, I then utilized a C library function [scandir()](http://man7.org/linux/man-pages/man3/scandir.3.html) in the header file `dirent.h` to create an extra function called [read_dir()](https://github.com/mapbox/tippecanoe/blob/master/tile-join.cpp#L344) in the tile-join code to recursively walk through the specified directory and its subdirectories to parse the path of all PBF files in the right order as in MBtiles.

However, another problem emerged during the testing of `read_dir()`. That is, the order of input directory might differ based on the use of operating system. As can be seen in Figure 2, the directory listing is in lexicographic order on my Mac OS X local machine, but in fact what we require is numerical order. As a matter of experience, the fastest solution is to use `versionsort()` function as the forth argument in `scandir()`. Unfortunately, it is only supported in GNU OS, so as an alternative, I used `alphasort()` function and a new integer variable `zoom_range` to handle the input in numerical order.

After solving these two tricky problems, I thought there would be no more obstacle ahead, but then a new issue arose. It seemed to be the culprit that the `d_type` field in [dirent structure](http://man7.org/linux/man-pages/man3/readdir.3.html) was only fully supported by some file systems such as Btrfs, ext2, ext3, and ext4 but not others, so it might return `DT_UNKNOWN` as undetermined type for a directory rather than `DT_DIR`. As a result, I used [lstat(2)](https://linux.die.net/man/2/lstat) system call in `read_dir()` function to avoid this potential risk.

After all the various adjustment for `read_dir()`, I started to create another new function called [dir_read_tile()](https://github.com/mapbox/tippecanoe/blob/master/dirtiles.cpp#L8) in the tippecanoe code to be able to read PBF data from its given path. On top of that, the `metadata.json` file in input directory has to be analysed and regenerated a new one later in the decoding stage of tile-join. At this moment, the tile-join input directory function was achieved. With the help of pointers, multiple input directories could also be accomplished. Straight afterwards, I exploited the no tile compression and output to directory function that I did earlier to enrich the options for tile-join.

Eventually, tile-join is now allowed to use a directory of tiles as source, and can output a directory of `.pbf` files instead of a `.mbtiles` file. Multiple source MBtiles files and directories of tiles are feasible as well. In addition, there is also a new option `-pC` for no tile compression which is the same as in tippecanoe. For instance, we can merge two directories of tiles `Road` and `NamedPlace`, and generate a directory of raw PBF files with this command:

```
tile-join -pk -pC -e Road_NamedPlace_Merged Road NamedPlace
```
Any combination of input MBtiles and directory is provided, and CSV file can also be used as the source for new attributes to join to the features. If we require an MBtiles output by merging `Road.mbtiles` and a directory of tiles `NamedPlace`, it is achievable with this command:

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

Tippecanoe directory support has been implemented with success, including the tile-join tool. It is beneficial to adopt directory structure and merging process for handling vector tiles, which can be further used for rendering like in [Tiler](/2017/05/14/tiler/) and [building your own static vector tile pipeline](/2017/05/19/build-your-own-static-vector-tile-pipeline/). It was such an experience to make contribution to Mapbox's Tippecanoe. We hope that this achievement can provide a method for anyone in the need of vector tiles.

Feel free to leave a comment if you have any question.

[Creative Commons Share-a-like](https://creativecommons.org/licenses/by-sa/2.5/) Joy Shan-Chun Kuo &copy; [Geovation](http://geovation.uk/)