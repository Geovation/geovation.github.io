---
category: tech
tags: notes
title: 4D changes with Mapbox GL JS
meta: A demo that shows 3D changes over time using Mapbox GL JS
author: Konstantinos Dalkafoukis
comments: true
---

A demo that shows 3D changes over time using Mapbox GL JS

## Introduction

This project shows [3D changes](https://geovation.github.io/changes-demo/) over time using Mapbox GL JS (v0.46.0). It's an evolved version of a [2D changes](https://demos.ordnancesurvey.co.uk/public/demos/products/changes/changes.html) project with  3D terrain and 3D buildings - allows you to move around, zoom and pan in & out.

## Library choice

### Mapbox GL JS is the uncontested winner

The size of the dataset demands webGL acceleration as it is impossible for the CPU to handle the onus. Two are the big players on the open-source community at the moment that support webGL;  
[Mapbox GL JS](https://www.mapbox.com/mapbox-gl-js/api/) and [Cesium](https://cesiumjs.org/).

The only way to create [3D Tiles](https://github.com/AnalyticalGraphicsInc/3d-tiles) for Cesium is [FME](https://www.safe.com/) but creates about 10 times(~150 MB for one zoom level compared to ~30 MB per frame that includes zoom levels between 12 and 16) bigger files compared to Mapbox's [Vector Tiles](https://github.com/mapbox/vector-tile-spec).

On the other side Mapbox offers [tippecanoe](https://github.com/mapbox/tippecanoe) an incredible open-source tool
that provides all the tools and flexibility that is required.

Mapbox is one-way for the needs of the current project.

## Specifications

* [ReactJS](https://reactjs.org/) : FrontEnd library
* [OS Terrain 50](https://www.ordnancesurvey.co.uk/business-and-government/products/terrain-50.html) : 3D terrain
* tippecanoe : tool for creating the Vector Tiles
* [rio-rgbify](https://github.com/mapbox/rio-rgbify) : tool for creating the 3D terrain
* zoom levels : 12 - 16
* number of sources : 52
* number of layers : 3
* dataset size : 883 MB for the Vector Tiles (zoom levels 12 - 16)

## Timelapse technique

The timelapse effect is implemented as a loop that changes the frames (layers) in a "fixed" period of time (setInterval with fixed interval = 2 sec).

![Image](/assets/changes_demo_diagram.svg)

## Challenges during the production

### 1. Flickering effect

Until version (v0.44.0) there was not a reliable way to guarantee that a layer has been rendered, something that was fixed in version (v0.45.0).
Past frames were disappeared before the new one was completely rendered, resulting a flickering effect.

### 2. Ghost buildings - Memory warnings

The most obvious way to create the timelapse loop is to add - show a frame and then remove the previous one, but it creates ghost buildings after going back in time and memory warnings.
The solution is to change the visibility instead of adding/removing the frames.

### 3. Keep the timelapse smooth after some iterations.

Frames could take more time to render than the fixed time of the loop and as a result after some iterations they will overload the CPU as there will be more than one frame at a single point of time. In order to avoid that and keep the performance no matter how many iterations will happen it's necessary to load the next frame only if the previous frame has completely been rendered.

### 4. Mapbox event 'data' doesn't fire always

'data' event would be used to check if a frame has been rendered.
Random zooming in & out could block the event from being fired and as a result could make the timelapse to stop due to the code for challenge 2. that blocks the next frame until the current frame has been rendered.
It was solved by creating a loop with requestAnimationFrame that manually checks for the frame instead of the native event.

## Other Optimisations

* Combining layers (from 11 initially to 3) to reduce render time according to [Mapbox strategies](https://www.mapbox.com/help/mapbox-gl-js-performance/#source-update-time) for improving performance.

```
render time = constant time
            + [ number of sources * per source time ]
            + [ number of layers * per layer time ]
            + [ number  of vertices * per vertex time ]
```

* Reducing the dataset size about 50% (from 1.78 GB to 883 MB) while keeping all the data for all zoom levels.

  * Removing needless attributes from the geojson files that were used to produce the vector tiles.

  * Reduce detail via tippecanoe but not very obvious with bare eye  
  `--no-tiny-polygon-reduction --low-detail 11 --minimum-detail 11`

The above commands reduce the detail in small zoom levels without noticeable differences (default 12) and guarantee that will not create any unwanted and ugly polygon aggregations.It could go up to 10 without obvious differences but wouldn't save more than extra 50MB.

## Future work

Two are the main ways to further optimise the 4D changes demo but with aesthetic changes in return.

* Fixed screen size instead of full screen, as when the size of the screen increases the rendered information becomes bigger with impact on the GPU.

* Further data preprocessing like building aggregations to reduce the amount vertices.

* Data hiding at lower zoom levels to keep the same information at every zoom level.

### A future project

From 2D changes over time to 3D changes to an [AR](https://www.mapbox.com/augmented-reality/) changes challenge.
