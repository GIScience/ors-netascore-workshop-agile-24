# Exploring Disaster-Aware Routing with the ORS Plugin for QGIS

Welcome to the hands-on part of our workshop on utilizing the ORS (OpenRouteService) tool plugin for QGIS, a powerful software combination that enables direct communication between QGIS and the OpenRouteService API. In this workshop, we will guide you through the process of setting up an account on openrouteservice.org, installing the plugin, and integrating your API key. Once set up, you will have access to a range of features, including directions, isochrones, and matrix analyses, which can be seamlessly accessed through the familiar QGIS interface.

While exploring the general functionality of the ORS plugin, we will specifically focus on its application in disaster-aware routing. To demonstrate its practical use, we will dive into the scenario of the devastating Ahr Valley flood that occurred in Germany in 2021. By leveraging the plugin's capabilities, we will learn how to optimize routing decisions in the context of disaster response and recovery.

By the end of this workshop, you will have gained valuable insights and practical skills in using the ORS plugin for QGIS, empowering you to make informed routing decisions and contribute to more efficient disaster management efforts. Let's embark on this journey of exploring disaster-aware routing with the ORS plugin and QGIS!


## Resources

Here is a list of resources on 

* How to sign up for an account
* How to setup up the ors tools plugin
* How to run directions and isochrones

Wiki:
[https://gitlab.gistools.geog.uni-heidelberg.de/giscience/disaster-tools/gis-in-anticipatory-humanitarian-action/-/wikis/ORSTools](https://gitlab.gistools.geog.uni-heidelberg.de/giscience/disaster-tools/gis-in-anticipatory-humanitarian-action/-/wikis/ORSTools)

PDF slides
slides: [ORS Tools step by step](hosted as pdf in the repo)

YouVideo [](https://www.youtube.com/watch?v=Rsxl_0IUSFM&t=12s)

Before we start with the training, make sure to download the datasets here:

[https://heibox.uni-heidelberg.de/d/6a9bcec375c64839b84f/](https://heibox.uni-heidelberg.de/d/6a9bcec375c64839b84f/)

* `affected_roads_bridges.gpkg` Road and bridge infrastructure damaged or destroyed by the flood
* `affected_places.gpkg` Communities affected by the flood in the Ahr valley represented as indvidual points.
* `affected_municipalities.gpkg` Municipal boundaries affected by the flood in the Ahr valley represented as polygons.
* `physicians.gpkg` Physicians located in and close to the Ahr valley represented as points.

## Single routes

### Task 1: Creating a Simple Route using the ORS Tools Plugin in QGIS

In this first task, we will focus on creating a single, straightforward route using the ORS Tools plugin within the QGIS software. Your objective is to define an origin/source and a destination, and utilize the plugin to generate a route between these two points. To get started, you can choose Nürburgring in Germany as the source location since it served as the staging area for disaster relief during the emergency response. For the destination, you can select Bad Neuenahr-Ahrweiler, the largest city affected by the Ahr Valley flood.

To accomplish this task, refer to the available resources provided to familiarize yourself with the functionality and features of the ORS Tools plugin. Explore the plugin's interface, study the options available, and apply the necessary settings to successfully generate the desired route. Feel free to experiment and troubleshoot any challenges you may encounter along the way.

By completing this task, you will gain a solid understanding of how to leverage the ORS Tools plugin in QGIS to create simple routes, setting the foundation for more advanced applications in subsequent exercises. Good luck, and enjoy the learning process!

IMAGE of the result

### Task 2: Incorporating Avoid Areas into the Route using the ORS Tools Plugin in QGIS

In this second task, we will build upon the knowledge gained from the previous exercise and introduce the concept of avoid areas in route planning using the ORS Tools plugin in QGIS. Your objective is to repeat the workflow of creating a route between the Nürburgring and Bad Neuenahr-Ahrweiler, but this time, you will add specific areas that should be avoided along the route.

To accomplish this, you will need to create a virtual polygon layer in QGIS, which will represent the avoid areas. Refer to the provided resources for detailed instructions on creating and managing virtual polygon layers. Once you have created the layer, you will adjust the corresponding parameter within the ORS Tools interface to account for the avoid areas during route calculation.

IMAGE of the result

## Multiple routes

### Task 3: Get regular and disaster aware routes

request without avoid

request with avoid

Load the `affected_roads_bridges.gpkg`. 
The damaged infrastructure is represented as lines. 
To be used in a openrouteservice avoid areas request, it needs to be either a polyon or multipolygon and the coordinates must be in the format of `EPSG:4326`. 
Therefore it is necessary to apply the following processing tools to the layer: __Reproject(EPSG:25832)__ >> __Buffer(2m)__ >> __Dissolve/Union__ >> __Reproject(EPSG:4326)__.
The resulting layer is to be used as input for the avoid polygons options in the ors tools directions processing tool.

IMAGE of the result


### Task 4: Postprocess routes


## Single Isochrones

### Task 5: Single Isochrone

### Task 6: Single Isochrone with avoid area

## Multiple Isochrones

### Task 7: 

### Task 8: Postprocess isochrones

Dissolve

Zonal statistic

single and with avoid areas

##

first task

routes vs disaster routes in 



How to run a local instance via docker
