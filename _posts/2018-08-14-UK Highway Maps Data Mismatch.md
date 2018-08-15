---
category: tech
tags: notes
title: UK Highway Maps Data Mismatch
meta: A script to find mismatched information of roads from different datasets providers to facilitate map updates and reduce the cost of gathering the information. The surveyors checking time can be reduce to only mismatched places.   
author: Dr Yousaf Omar Azabi
comments: true
---

A project initiated for reducing the costs and time of updating UK highway maps of Ordnance Survey.

## Introduction

The advance in technology and the increase demand on using geospatial data in diversity of projects in public transportation, environment, oil & gas, housing, etc, has led to a variety of providers of geospatial data ranging from government agencies, companies and open-source communities, such as [Ordnance Survey (OS)](https://www.ordnancesurvey.co.uk), [Google Maps](https://www.google.com/maps), and [Open Street Map (OSM)](https://www.openstreetmap.org) to name a few. The highway roads information is continuously changing and these changes lead to mismatches in databases due to time taken to figure changes and update databases.

In this [project](https://github.com/Geovation/roads) a planed procedure is proposed to analyse and find the mismatches between databases from different sources to provide a valuable information for surveyors so that mismatched information can be investigated and checked to update databases more efficiently.

## Aim

The [OS](https://www.ordnancesurvey.co.uk) surveyors used to check for highway roads mismatches in a random way due to the lack of up to date sources of information. Therefore, [OS](https://www.ordnancesurvey.co.uk) surveyors and [Geovation Hub](https://geovation.uk) decided to provide a solution to increase the efficiency of the surveyors visits to places of mismatches by comparing [OS](https://www.ordnancesurvey.co.uk) data with other geospatial data from different providers. This will provide a list of mismatched information that can be checked to improve the [OS](https://www.ordnancesurvey.co.uk) maps and keep them up to date.

## Proposal

The project involves various stages to develop a comprehensive procedure that can help to figure out all the various mismatched information (one-way direction, turning restrictions, no entry, and so forth). The stages of the proposed project are listed below:
* Find mismatch in the directionality of one-way roads between [OS](https://www.ordnancesurvey.co.uk) an [OSM](https://www.openstreetmap.org) data.
* Find other mismatches in both data.
* Compare data from other providers.
* Design a user interface to implement the scripts.

## Technology Required

* [Geospatial Data Abstraction Library (GDAL)](https://www.gdal.org/index.html) : library for preprocessing geospatial data.
* [JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript) : language used to write the script.
* [turfJS](http://turfjs.org) : JavaScript library for processing geospatial data.
* [ReactJS](https://reactjs.org) : to develop the User Interface (UI).

## Challenges

The information is saved as layers in databases therefore an understanding of which layer has the information of interest is required to extract the required layer.

Roads are saved as segments (links) in the geospatial databases. Almost every road consists of more than one segment, so that database contains various record entries of the same road. Each database uses different numbers and length of segments to represent roads. Furthermore, the coordinates are misaligned due to differences in points measured along the link and errors in GPS inquired data. These errors leads to a deviation in position of the roads obtained from diverse databases.

The information of links saved in databases using different keys and information tags. Extract these information from databases to compare similar characteristics requires analyses and understanding of the structure of the data.

The data size is large and a procedure to deal with huge amount of data is required.

## Methodology

The data is acquired from the websites of [OS](https://www.ordnancesurvey.co.uk) and [OSM](https://www.openstreetmap.org). The data is preprocessed using the [GDAL](https://www.gdal.org/index.html) line command [`ogr2ogr`](https://www.gdal.org/ogr2ogr.html) to:
* Choose the required layer form data.
* Convert data format to JSON.
* Convert map projection system.
* Crop the map to the specified area.
* Filter data using SQL query.

Choosing the required layer should be run first. The other steps can be interchangeable. However, all the steps can be merged in one command line to process the data. For more details on how to implement the [`ogr2ogr`](https://www.gdal.org/ogr2ogr.html) click [here](https://github.com/Geovation/roads/blob/master/InputY/README.mds).

The data obtained is then processed further using a script to check validity of links coordinates.

### The first task is comparing road links from OS and OSM and find a possible one match for every link from OS in OSM.

The data processed is used as input for the script to find roads matches using the road name and coordinates. The name is compared first and when a match is found, the coordinates (position of roads) is compared using function [`lineOverlap`](http://turfjs.org/docs/#lineOverlap) from library [`turfJS`](http://turfjs.org). The function takes two required inputs of type [`lineString`](http://turfjs.org/docs/#lineString) and an optional input of type number as distance tolerance.
The match possibilities are; no-match, one-match, multi-match or a road without a name. The output of one-match required to be highest. The tolerance have been varied to find the optimum length as shown in the graph.

![Image](/assets/roads/vary-tolerance.svg)

**Figure 1** - Varying the tolerance of the function [`lineOverlap`](http://turfjs.org/docs/#lineOverlap) to find optimum length.

### The second task is comparing one-way roads and find directionality mismatch.

The links matches from previous task are compared to find one-way roads and then calculate the direction using coordinates. The angle of the link is calculated using the start and end coordinates. The difference between angles of both roads is set to be greater than 10 degrees to consider a mismatch.

The script has been tested on small area and London. The results shows some errors due to different factors:
* Road splits to two branches with similar name and the distance in between is small. The script could identify a road match for different sides of the road due to separation distance is small (Figure 2). This can be minimised by reducing distance tolerance but should not go for very small values of less than 2m.

![Image](/assets/roads/round-about.png)

**Figure 2** - Schematic of overlap error when road split with small distance.

* At junctions, links from different road direction may overlap at small portion (Figure 1). This can be overcome by considering links with overlap length over link length to be greater than 0.5.

![Image](/assets/roads/road-junction.svg)

**Figure 3** - Schematic of overlap error at road junction.

* One-way roads with buses lanes in the opposite direction. This can be overcome by checking motor vehicles are allowed or not.

## Future work

The project is still in the development stage and next task is to include mismatch of other road features. This would involve much effort to analyse the features from both databases and extract mismatches.

The second phase is to compare with various databases to improve the efficiency of the output. This phase would involve analysis of the new databases and manipulating the script to fit with these databases.

The final phase is to produce a UI web app for the surveyors to use the script on the run to update the mismatches. This can be done in parallel while working in first and second phase.
