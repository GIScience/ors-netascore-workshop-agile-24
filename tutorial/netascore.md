# NetAScore

# Installing and using NetAScore
## Pre-requisites and Installation
To run NetAScore on your machine using the ready-made Docker image, you need to have [Docker](https://docs.docker.com/engine/install/) installed. Before you start with the Tutorial, make sure Docker is running and that you have a stable Internet connection.

### 1. Get the resources
After you installed Docker, you need to download the the [`docker-compose.yml`](https://github.com/plus-mobilitylab/netascore/blob/main/examples/docker-compose.yml) raw-file from the `examples` and save it to an empty directory, in this tutorial the directory is called `NetAScore`. 

If you have not run NetAScore before, you need to create a new subdirectory called `data` within the folder created previously.
Proceed with downloading the settingstemplate from GitHub ([`examples/settings_osm_query.yml`](https://github.com/plus-mobilitylab/netascore/blob/5099d4a7ff6a2c348fe39dd9f0dfb8b8ba973c32/examples/settings_osm_query.yml)), and save it to the subdirectory. 
Lastly, you need to download the mode profiles for _bikeability_(`profile_bike.yml`) and _walkability_ (`profile_walk.yml`)and save them to the data directory as well.


### 3. Let's adapt the default profile for your own area of interest
In order to run NetAScore for your own area of interest, you need to adapt the *settings file* (`settings_osm_query.yml`). To do so, you first change the case_id and then specify the name of the place you want to run NetAScore for.

The `case_id` located in the `global` section of the file and is a unique ID used to distinguish between different datasets (only alphanumeric characters are allowed). 

The `place_name` is used to determine the area, the OSM data is queried for and can be changed in the `import`-section of the file.
To specify your area of interest, change the variable of `place_name` to the name of the place you want to calculate the _bikeability_ and _walkability_ for.

![how to change the place_name](https://github.com/plus-mobilitylab/netascore/wiki/resources/img/change_place_name.png)

If you want to know more about the adjustments that can be made within the settings file, feel free to have a look at the [wiki page](https://github.com/plus-mobilitylab/netascore/wiki/Configuration-of-the-settings), that deals with further configuration of the settings.

### 4. Let's run Netascore!
After the adjustments to the settings file, we can now compose the Docker image.
To do so in the terminal, first navigate back to the Parent-folder you created, in our case `NetAScore`, and enter the following command line prompt:

```
docker compose run netascore data/settings_osm_query.yml
```
Docker will now start downloading the NetAScore image and the PostgreSQL database image, setup the environment and execute the workflow for your area of interest. (The last argument represents the settings file to use. Note that this is the general way of specifying the settings file that should be used.) 

Running the docker image might take a while, as NetAScore first loads an area of interest by place name from Overpass API, then downloads the respective OpenStreetMap data and afterwards imports, processes and exports the final dataset. You can tell the run was completed successfully once the Database connection is closed, which looks like this:

![image of successful run](https://github.com/plus-mobilitylab/netascore/wiki/resources/img/cmd_complete.png)

You can now find the assessed network as a geopackge within the `data`-sub-folder you previously created. It includes _bikeability_ in columns `index_bike_ft` and `index_bike_tf` and _walkability_ in `index_walk_ft` and `index_walk_tf`. The extensions `ft` and `tf` refer to the direction along an edge: from-to or to-from node. These values represent the assessed suitability of a segment for cycling (_bikeability_) and walking (_walkability_).

If you want to run NetAScore for another area of interest, do not forget to change the `case_id`, otherwise your previous file will be overwritten.

_NetAScore allows you to directly download OpenStreetMap data via Overpass API. If you want to provide a local OSM-Export you can do so using the 'filename' property instead of the 'place_name' that is given in the 'settings_osm_query.yml'. In some cases you might want to specify your search, if the place name given is not unique (e.g. Frankfurt (Main) & Frankfurt (Oder)). To accomplish a specification you can either specify additional parameters:_

* `admin_level`: filters the given results for OSM `admin_level` property (see [OSM documentation](https://wiki.openstreetmap.org/wiki/Item:Q1074))
* `zip_code`: filters the given results for a ZIP code (if available in OSM data)

Another option is to set the `interactive`- Variable to `interactive: True`, to opt for an interactive prompt.

**_Please note: Network data is being queried based on the bounding box (rectangle) containing the polygon returned for the place name query. If you do not specify a reference system (global option target_srid), the UTM zone suitable for the centroid of the area of interest is used._**

For further information on additional options for OSM see the [settings documentation](https://github.com/plus-mobilitylab/netascore/wiki/Configuration-of-the-settings#additional-options-for-osm).

### Let's visualize your results!

Finally, we want to visualize the data, we will do so using QGIS. To visualize the assessed network, you can simply drag and drop the `.gpkg`-file into QGIS and import the line-layer.
You can now see the line segments within your area of interest, they are however the same colour by default, which means we cannot differentiate between different index values. The index values range from 0 (unsuitable infrastructure) to 1 (very suitable infrastructure).

To differntiate between the values, 


# What is NetAScore?
NetAScore provides a toolset and automated workflow for computing bikeability, walkability and related indicators from publicly available network data sets. Currently, we provide common presets for assessing infrastructure suitability for cycling (_bikeability_) and walking (_walkability_). By editing settings files and mode profiles, additional modes or custom preferences can easily be modeled.

For global coverage, we support *OpenStreetMap* data as input. Additionally, Austrian authoritative data, the *'GIP'*, can be used if you work on an area of interest within Austria.

NetAScore ...

- ... is a (commandline) software tool for assessing infrastructure suitability for different modes
- ... provides default profiles for cycling and walking
- ... can be used as foundation for advanced network analyses e.g. in urban planning
- ... supports OpenStreetMap and GIP (for Austria) network data
- ... is open source


# How to use NetAScore
If you want to run NetAScore you can choose between two modes, you can either use our ready-to-use setup that utilizes a docker image or you can run it manually using Python.

### ... using Docker
NetAScore comes with a `docker compose` configuration in `docker-compose.yml` and a demo configuration, so you can simply run an example workflow by following the tutorial presented on this page or by following the [guide on our GitHub-page](https://github.com/plus-mobilitylab/netascore/wiki/How-to-run-the-project-in-a-Docker-environment). The great advantage of running NetAScore in Docker is that you do not have to install any software or python dependencies on your machine.

### ... on your local machine using Python

If you prefer to execute NetAScore directly on your machine, virtual environments or Conda may help with managing all Python requirements. You can learn how to run NetAscore manually on your local machine by reading the [guide on our GitHub-page](https://github.com/plus-mobilitylab/netascore/wiki/Run-NetAScore-manually-with-Python).
