# openrouteservice

## Installation & Prerequisites

- [Docker](https://www.docker.com/)

The simplest way of running openrouteservice is through Docker and Docker Compose. To install these on your system, 
you can follow the guides provided by Docker at ...

### 1. Getting the resources

The very first thing that you need to get openrouteservice running is to get the Docker Compose file. You can create 
this yourself, but the recommended method is to download one that has been automatically created for the latest 
version of openrouteservice. This file is available in the Assets part of the latest release page 
(https://github.com/GIScience/openrouteservice/releases/tag/v8.0.1). Whilst you ar there, it is also a good idea to 
download the configuration file `ors-config.yaml`. Openrouteservice can have settings applied through this 
configuration file or through environment variables, but in this tutorial we will only work with the yaml configuration.

You should also download some OSM data that you can use in your local instance. One thing to keep in mind when 
choosing the area you want ot use is how much data is there. The more data there is, the more RAM and time needed 
for building the routing graph. As a general rule of thumb, we say that the amount of RAM needed **per profile** is at 
least twice the size of the OSM pbf file, meaning if you wanted to build the car profile for Germany, which has a pbf 
file size (from GeoFabrik) of 4.0GB, you would need to allocate at least 8.0GB of RAM. A good place to download OSM 
data from (as already mentioned) is GeoFabrik. From their website (https://download.geofabrik.de) you can download 
various extents of OSM data split by continent, country or region. If you want a smaller region than what they offer,
then you can also use tools such as Osmium to extract regions from pbf files. For this tutorial, we have provided a 
pbf file of Glasgow, which you can access from the resources page of this workbook.

### 2. Setting up the file structure
If you are using the docker compose file provided by openrouteservice (see previous step), then we will be working 
with Docker volumes mapped to folders. These mappings basically link the folders of the host machine to folders 
inside the Docker container.

For this tutorial, the file structure is as follows:

```
openrouteservice
│   docker-compose.yml
└───config 
│       ors-config.yml  
└───elevation_cache
└───files
└───graphs
└───logs
```

To start with, create a folder called `openrouteservice` which will act as the root of the openrouteservice folder 
structure. Inside this folder you should put your `docker-compose.yml` file and create subfolder which will then be 
mapped into the Docker container. These folders will then be used to pass data into openrouteservice, as well as 
preserving data generated. The folders needed are:

* **config** - this will be where you store the configuration file for openrouteservice
* **elevation_cache** - when you tell openrouteservice to take into account elevation data, this data is downloaded from 
  various sources. To make sure that it is only done once (as long as you don't change the area covered by the pbf 
  file), files are mapped into the elevation_cache folder so they are preserved when the container is shut down
* **files** - the `files` folder is used ot pass data into the container. Such files include the OSM data, and any 
  other data used for graph building (e.g. a walkability index csv file)
* **graphs** - openrouteservice builds routable graphs based on the OSM data. These graphs are stored to disk and 
  comprise of multiple files representing different aspects of the data created. By mapping this folder, the graphs 
  are preserved after the container is shut down
* **logs** - during the build and serving of graphs, openrouteserivce creates log files relating to what is 
  happening and any errors. These are stored in this folder

For the basic setup, you should place the ors-config.yml file in the `config` folder, and the pbf file in the 
`files` folder.

### 3. Configuration

Before configuring openrouteservice itself, you need to setup the docker compose environment. This is done through 
the `docker-compose.yml` file. When you use the file provided by openrouteservice, the important aspects that need 
changing before running openrouteservice are the mappings of volumes and the amount of RAM to allocate. Under the 
`volumes` section of the file, you will see a number of commented out (with a #) lines. These are the mappings to 
the folders that we created in teh previous step, and so to allow the mappings you need to uncomment them (remove 
the first # of each line). You also need to comment out (add a # at the start of the line) the first mapping. Int eh 
end, the volume section should look as follows:

``` yaml
volumes:  # Mount relative directories. ONLY for local container runtime. To switch to docker managed volumes see 'Docker Volumes configuration' section below.
      # - ./ors-docker:/home/ors  # Mount the ORS application directory (for logs, graphs, elevation_cache, etc.) into its own directory
      - ./graphs:/home/ors/graphs  # Mount graphs directory individually
      - ./elevation_cache:/home/ors/elevation_cache  # Mount elevation cache directory individually
      - ./config:/home/ors/config  # Mount configuration directory individually
      - ./logs:/home/ors/logs  # Mount logs directory individually
      - ./files:/home/ors/files  # Mount files directory individually
``` 

Next, we may need to edit the amount of RAM allocated. Again, the minimum amount of RAM is generally at least twice 
the size of the pbf file. You change this with the `XMS` and `XMX` properties (minimum and maximum size of the Java 
heap respectively). Once you have modified these parts of the docker-compose file, you are ready to move on to 
modifying the configuration of openrouteservice itself.

Configuration of openrouteservice is handled via the ors-config file (at least in this tutorial). If you take a look 
inside this file, you will see a lot of commented out lines which give examples of some settings that you can change. 
Right now, the most important setting is which OSM file to use for building graphs. About one third down this file 
you will see the property `source_file`, which by default is set to `ors-api/src/test/files/heidelberg.osm.gz`. This 
is a small sample file provided by openrouteservice for Heidelberg in Germany, but we want to use our own OSM file. 
As we are mapping the `files` folder to `/home/ors/files`inside the container, we need to change that property to 
match. So if we have an OSM file called `my-osm-data.osm.pbf` inside the `files` folder, we change the property in 
the config file to be `source_file: /home/ors/files/my-osm-data.osm.pbf`. The important thing to note here is that 
the path is for the file **inside of the container**, hence why use the `/home/ors/files` rather than the path that 
is on our local machine.

Once you have changed that setting, you are good to go to build your first openrouteservice instance! 

### 4. Starting your first instance

To start your own openouteservice instance, all you need to do is to use docker compose. From the same folder as the 
`docker-compose.yml` file, open a terminal window and execute `docker compose up -d` (you may need to do this as a 
sudo command depending on your system's Docker setup). The `up` part tells docker compose to start the containers 
listed under the `services` part of the docker-compose file and any relevent networks. The `-d`tells docker compose 
to do this in detached mode, meaning that it is running in the background. If you execute `docker ps` you should see 
a container running called ors-app.

You can follow what is happening in the build process by viewing the docker logs using `docker logs -f ors-app`. The 
`-f` means "follow" so any new log information will be shown on the terminal. If there are no errors, after some 
time you should see something similar to the following in the log output:

```
ORS-pl-walking [ o.h.o.r.g.e.ORSGraphHopper         ]   flushed graph totalMB:4644, usedMB:3653)
ORS-pl-walking [ o.h.o.r.RoutingProfile             ]   [1] Edges: 1474291 - Nodes: 1208923.
ORS-pl-walking [ o.h.o.r.RoutingProfile             ]   [1] Total time: 150.787s.
ORS-pl-walking [ o.h.o.r.RoutingProfile             ]   [1] Finished at: 2024-05-15 11:58:41.
ORS-Init [ o.h.o.r.RoutingProfileManager            ]   Total time: 305.783s.
ORS-Init [ o.h.o.r.RoutingProfileManager            ]   ========================================================================
ORS-Init [ o.h.o.r.RoutingProfileManager            ]   ====> Recycling garbage...
ORS-Init [ o.h.o.r.RoutingProfileManager            ]   Before: Total - 4.54 GB, Free - 990.69 MB, Max: 8 GB, Used - 3.57 GB
ORS-Init [ o.h.o.r.RoutingProfileManager            ]   After: Total - 4 GB, Free - 3.26 GB, Max: 8 GB, Used - 759.19 MB
ORS-Init [ o.h.o.r.RoutingProfileManager            ]   ========================================================================
ORS-Init [ o.h.o.r.RoutingProfileManager            ]   ====> Memory usage by profiles:
ORS-Init [ o.h.o.r.RoutingProfileManager            ]   [1] 287.03 MB (37.8%)
ORS-Init [ o.h.o.r.RoutingProfileManager            ]   Total: 287.03 MB (37.8%)
ORS-Init [ o.h.o.r.RoutingProfileManager            ]   ========================================================================
```

Don't worry if the numbers are different - that all depends on how much RAM has been allocated and how big the osm 
files are. The important thing is that you see the `[1] Finished at:` part as that indicates that building/loading 
of graphs has finished

You can also see whether the instance is ready to receive requests (it has built and/or loaded graphs) by calling 
the openrouteservice API `health` endpoint. When you execute `curl localhost:8080/ors/v2/health` you will either get 
a response of `status: ready` or `status: not ready` depending on if it is ready or not. When you see `status: 
ready` then openrouteservice is up and running and ready to receive routing requests. 

CONGRATULATIONS! You now have your very own openrouteservice instance!

### 5. Querying your instance

Now that you have your instance up and running, you can make queries against it. As we used the (pretty much) 
default configuration, the only profile active is the car profile. We will cover how to enable different profiles in 
the next section.

A great place to start for building queries is the openrouteservice interactive API docs, which you can find at 
https://openrouteservice.org/dev/#/api-docs. On there, you can construct queries using different parameters (note 
that not all are available in the default setup), and then if you click on the `Example code` button, you will be 
given the curl request for that combination. You can then copy that and use it as the basis for the request against 
your local instance. The main thing that will need changing though is the base url as by default it points to the 
global API.

``` bash
  curl -X POST \
  'http://localhost:8080/ors/v2/directions/driving-car/geojson' \
  -H 'Content-Type: application/json; charset=utf-8' \
  -H 'Accept: application/json, application/geo+json, application/gpx+xml, img/png; charset=utf-8' \
  -d '{"coordinates":[[8.681495,49.41461],[8.687872,49.420318]],"preference":"shortest"}'
```

In the above example, the difference between the call to the loca and the global instance is the URL - 
`http://localhost:8080/ors/v2/directions/driving-car/geojson` rather than 
`https://api.openrouteservice.org/v2/directions/driving-car/geojson` (you also don't need to provide an API key in 
requests to local instances).

Now that you know how to query the instance, feel free to play around with generating routes, isochrones, and matrix 
requests. You can use the interactive documentation to produce a wide variety of requests that you can make, just 
remember that currently you can only do calls to the car profile

### 6. Using different profiles

So now you have an instance running and you can make queries, but only to the car profile. Let's change that!

Getting other profiles built in the docker openrouteservice setup is as simple as modifying a couple of lines in the 
ors-config.yml file, and then recreating the openrouteservice Docker container. So, let's turn on the walking 
profile (seeing as we are in a tutorial about walkability). 

The first thing to do is to open up the ors-config.yml file for editing. As you scroll down (past the huge chunks of 
commented out configurations) you will come across the `profiles` section (roughly halfway). This is where we define 
the profiles that graphs are built for and can subsequently be routed against. As you can see, under this there is 
the `car:` section, which is what tells openrouteservice to build the car profile. Under this section there are a 
number of configuration properties such as `preparation: -> methods:` which define the algorithms that are run to 
build and query the graphs. We don't need to think about these just yet, but it's good to know that they are there. 
The part that we are currently interested in start at the `# walking:` line. As you can see, this is commented out 
and so openrouteservice in effect doesn't know anything about building the walking profile. If we remove the #, the 
line is now active. By doing that, when we run openrouteservice with this configuration file, it will know to build 
the walking graph. So let's do that.

For any changes made in the config file to take effect, we need to recreate the openrouteservice Docker container. 
To do that, we need to be in a terminal window in the root folder of the openrouteservice setup (where your 
`docker-compose.yml` file is) and execute the command `docker compose down`. This shuts down the openrouteservice 
container, but as we are using volumes mapped to folders, the graphs generated for the car profile are persisted and 
will be loaded into the container when we restart it, so those won't need rebuilding.

This is a good spot to point out that if you ever need to rebuild graphs (e.g. if you change the osm data) then you 
need to delete the corresponding graphs folder. For example, if we wanted to rebuild the graphs for the car profile, 
we would need to delete the `./graphs/car` folder on the host machine.

As we are increasing the number of profiles that are being run, we also need to make sure that there is enough RAM 
available. You can change the values in the `docker-compose.yml` file as described in section 3. So if you are 
building two profiles on a 1GB pbf file, you will need to allocate at least 4GB (2GB for car, 2GB for walking).

OK, so now openrouteservice has shut down. we need to restart it again with the same `docker compose up -d` command. 
Again you can watch the logs with `docker logs -f ors-app` and when you see that the profiles have finished loading, 
you can start querying. Though you are loading in the car graphs so they won't be built again, the walking graphs 
will need to be built so it may take a little time.

When building has finished, you can make queries against both the car and the walking profile as described in the 
previous section.


### 7. Let's do some walkable routing

So now you know how to setup openrouteservice, how to use different OSM data, and how to enable different profiles. 
The final part of this tutorial is to do some routing which takes into account walkability.

Within openrouteservice, there are things called "extended storages" which are basically additional attributes that 
are added to the routing graph. Normally, the information stored in teh graph itself are just the distance between 
two nodes, and how long it takes to travel that distance. When you do routing that needs to take into account things 
such as walkability or avoiding things such as kerbs over a certain height or crossing country borders, that is where 
extended storages come into play. Some storages allow passing in extra data which are then matched to edges in the 
graph when the graph itself is built, such is the case with walkability. In this example, we will make use of a generic 
extended storage in openrouteservice called the csv storage (named so because data is passed to openrouteservice in 
the form of a csv file). This storage applies additional weightings to edges in the graph which then affect how much 
each edge costs. 

Firstly, lets take a quick look at the structure of a csv file that can be used in the storage:

| osm_id | bike_index | walk_index |
|--------|------------|------------|
|672899942|0.165|0.647|
|334525|0.873|0.811|

There are a few rules that need to be taken into account for the csv file. Firstly, the first column is the OSM id 
of the way that the value refers to. Secondly, you can have any number of additional columns, but the values need to be 
between 0 and 1, with 0 indicating that the way is completely suitable, and 1 that it is completely unsuitable.

> **_NOTE:_** Currently, there are some additional restrictions in the csv file that need to be taken into account, 
> though they will be eased up in future releases as the feature becomes more stable:
> * You cannot include any quotation marks (`"`) in the file
> * Any empty cells need to be marked as `NaN` (again without quotation marks) - `NULL` or empty cells are not permitted
 
OK, so now you know a little about the csv extended storage, it is time to put it to use. As you should make sure 
that the OSM ids in the csv file correspond with OSM ids of the pbf file that you use to build the graphs, we have 
provided an OSM pbf dile and corresponding walkability file for Glasgow which you can download from the resources 
section. Once you have downloaded them, put them both in the `files` folder of the openrouteservice setup, and 
delete any folders and data that are in the `graphs` folder. Your file structure should not look like the following:

```
openrouteservice
│   docker-compose.yml
└───config 
│       ors-config.yml  
└───elevation_cache
└───files
│       glasgow.osm.pbf
│       glasgow_walkability_index.csv    
└───graphs
└───logs
```

Now that the data is ready, take a look at what the headers of the csv file contain by using the command `head 
files/glasgow_walkability_index.csv` - you will need to know the column name of the walkability index value for when 
you query to get results. In our case, the column is called `walk_index`.

To make use of this data in the graph building, you need to modify the `ors-config.yml` file. Change the `walking` 
section under profiles to be the following:

```yaml
      walking:
        ext_storages:
          csv:
            filepath: /home/ors/files/glasgow_walkability_index.csv
```

and the osm source file to be:

```yaml
    source_file: /home/ors/files/glasgow.osm.pbf
```

Now all you need to do is stop openrouteservice if it is still running (`docker compose down`) and then start it up 
again (`docker compose up -d`). Once it has finished building, you will be able to make requests for walking routes 
that take into account the walkability.

To request a walkable route, you do the same as previously, but in addition to the starting and end coordinates, you 
also pass in some additional parameters. This is where the column name is needed:

```bash
 curl -X POST 'http://localhost:8080/ors/v2/directions/foot-walking/geojson' \
  -H 'Content-Type: application/json; charset=utf-8' \
  -H 'Accept: application/json, application/geo+json, application/gpx+xml, img/png; charset=utf-8' \
  -d '{"coordinates":[[-4.3121, 55.8826],[-4.2915, 55.8427]],"instructions": false, "extra_info":["csv"],"options": {"profile_params": {"weightings": {"csv_column": "index_walk","csv_factor": 1.0}}}}'
```

The first addition is the `"extra_info":["csv"]`. This adds some extra information in the response from 
openrouteservice that shows the values obtained from the csv storage for segments along the route (we will talk 
about that shortly). The part that actually tells openrouteservice to use walkability in routing though is the 
`"options": {"profile_params": {"weightings": {"csv_column": "index_walk","csv_factor": 1.0}}}}` part. Here we are 
telling openrouteservice to use the csv_column called `index_walk` when calculating a route, and that we should 
apply the maximum effect from it (the `1.0`). This means that when an edge in the graph is evaluated, the base value 
from OSM is taken, then increased by the full factor of the walk index from the csv (that is why 0 in teh file is 
seena s most suitable). In practice, that means if the base weight of the edge was 100, and it was from an OSM way 
that had a walkability index value of 0.8, when we pass 1.0 as the csv_factor the end weight of the edge would be 
180 (`100 + ((100 x 0.8) x 1.0)`). If we passed in 0.1 as the csv_factor, the end value would be 108 (`100 + ((100 x 
0.8) x 0.1)`). This means that the lower the csv_factor value, the less the walkability factor has an effect on the 
route generated.

To look at differences between routes generated, you can either save the files as geojson and load them into a 
visualiser, or copy the geojson response body and paste it into a websites such as https://geojson.io.

As mentioned, another part of the returned route from the above example is the "extra information". As we requested 
to receive csv data, alongside the route geometry we get some values stored in a two dimenstional array, which looks 
something like the following:

```json
{"extras":{"csv":{"values":[[0,1,49],[1,2,33],[2,5,53],[5,6,46], ... ]
```

These arrays of three numbers represent sections of the route that have a specific value that was obtained from the 
csv file - in this case the walkability index (when requesting csv extra info, the value returned is for the column 
that was specified as the `csv_column` in the request). In the case of the csv extra info, the value is always from 
0 to 100, and is the original csv file value multiplied by 100. In the above snippet, this means that between the 
first and second vertex of the geojson linestring, there was a walkability value of 49, which would have been 0.49 
in the csv file. Between vertices 2 and 3, the value is 33, and between vertices 3 and 6 ir is 53. Remember that a 
lower value means that a segment is **more** suitable!

## Next steps

That pretty much covers the basics of setting up openrouteservice on your own system and then querying it to get 
some routing results. At this point, feel free to keep playing around with different settings in the configuration 
file, or even installing openrouteservice through different methods. You can find general information about 
installing on the documentation at https://giscience.github.io/openrouteservice/run-instance/. There you can also 
find out about what different configuration parameters do.



