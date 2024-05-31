

### 1. Initialization

#### Docker

Follow the [manual/documentation](https://giscience.github.io/openrouteservice/run-instance/running-with-docker) to create the folder structure and files needed for a local ORS docker instance:

```shell
mkdir -p ors

mkdir -p ors/ors-docker/config ors/ors-docker/elevation_cache ors/ors-docker/files ors/ors-docker/graphs ors/ors-docker/logs

wget https://github.com/GIScience/openrouteservice/releases/download/v8.0.0/docker-compose.yml -O ors/docker-compose.yml
```

### 2. Setup new virtual environment with all necessary dependencies

It is advised to create a new virtual environment and install the necessary dependencies using a given file.

#### With pip

```shell
python3 -m venv csv-ors-builder

source csv-ors-builder/bin/activate

pip install -r requirements.txt
```

After this, you should be done.

#### With Mamba + Poetry

```shell
mamba env create -f environment.yml

mamba activate csv-routing

poetry install
```

Poetry will detect and respect an existing virtual environment that has been externally activated and will install the dependencies into that environment.

To update the packages to their latest suitable versions (and the poetry.lock file), run:

```shell
poetry update
```

### 3. Data

Put both of the following files in `ors/ors-docker/files`.

#### OSM file

[OSM file](https://giscience.github.io/openrouteservice/run-instance/data#osm-data) to build the graph on. File formats can be looked up [here](https://giscience.github.io/openrouteservice/run-instance/configuration/ors/engine/) under 'source_file'. The OSM file should include the OSM ids included in the following CSV file.

#### CSV data

The additional data you want to integrate in the ORS route calculation. Should be comma-separated, including at least one 'osm_id' column and a column with numeric data between 0 and 1

**Notice:** the algorithm uses a **malus system** for the value range 0 to 1, which means higher values are worse and lower values are better. The algorithm calculates costs and routes based on this.

If you want it the opposite way (**bonus system**), you have to recalculate your values in the CSV file. We provide a simple preprocessing tool taking care of this, which you can execute with:

```shell
python src/csvfile_preprocessing.py path-to-csv-file
```

### 4. Check engine vs. endpoint profile

**Attention!** There are two profiles, one for the engine during the graph building, and one for the endpoint where you send requests to.

- Check your **engine** profile [here](https://giscience.github.io/openrouteservice/run-instance/configuration/ors/engine/profiles)  
- Check your **endpoint** profile [here](https://giscience.github.io/openrouteservice/api-reference/endpoints/directions/extra-info/#extra-info-availability)

In the following configuration, the **engine profile** is used. Afterwards, when sending requests, you should switch to the according endpoint profile.

## Docker configuration

The only thing you have to change is the `ors/docker-compose.yml` file which has been downloaded before. Add the following configs at the according lines:
  
```yaml
services:
    ors-app:
        container_name: <your custom container name>
        environment:
            # should be the path to source file specified above
            ors.engine.source_file: /home/ors/files/<your custom osm file.pbf>
            # adjust to your engine profile
            ors.engine.profiles.<your engine profile>.enabled: true
            # add path to csv data
            ors.engine.profiles.<your engine profile>.ext_storages.csv.filepath: /home/ors/files/<your csv data.csv>
            # keep this and don't change
            ors.engine.graphs_root_path: /home/ors/graphs
            ors.engine.elevation.cache_path: /home/ors/elevation_cache
```

The environment variables could also be [included](https://giscience.github.io/openrouteservice/run-instance/running-with-docker#configure) in the custom-ors-config.yml, but for a simple addition of csv data to the graph, this minimal setup is enough. Additional options are explained [here](https://giscience.github.io/openrouteservice/run-instance/configuration/#file-content).

## Build local ORS instance

To build the image and create the container, use this:

```shell
docker compose -f ors/docker-compose.yml up -d
```

The graph is now build. To see when the ORS instance is ready to take requests (it takes around 5 minutes to build the graph for Austria), use this:

```shell
curl -s http://localhost:8080/ors/v2/health | jq -r '.status'
```

When it changes from `not ready` to `ready`, you can start sending requests.

## Query routes

The basic functionality to request routes from ORS is explained [here](https://openrouteservice.org/dev/#/api-docs/v2/directions/{profile}/geojson/post).

For a more elaborate example, take a look into these notebooks:

- [routing_validator.ipynb](./src/routing_validator.ipynb): validate if the routing is working by requesting n random routes and doing a simple evaluation
- [compare_routes.ipynb](./src/compare_routes.ipynb): query single routes in an interactive map and get both the default and the csv-optimized route with additional statistics

### Address to query local ORS API

Because you want to use a local ORS instance, you have to adjust the API address to a localhost address. For this, replace `https://api.openrouteservice.org` with `http://localhost:<port>/ors/`.

The port is specified in the [docker-compose.yml](./ors/docker-compose.yml) file under `services.ors-app.ports`, the one where the ORS API is exposed (default port 8080).

So the same endpoint for the online and local ORS API looks like this:

```shell
https://api.openrouteservice.org/v2/directions/<your endpoint profile>/geojson

http://localhost:8080/ors/v2/directions/<your endpoint profile>/geojson
```

### Interpretation of extra info

This is explained [here](https://giscience.github.io/openrouteservice/api-reference/endpoints/directions/extra-info/#extra-info).

Notice the used [malus system](#csv-data) when calculating the routes with the CSV values as cost calculation basis!

## Troubleshooting

**Error:** '... external connectivity on endpoint ... proxy listen tcp4 0.0.0.0:xxxx bind:address already in use'  
**Solution:** either close port connection (kill process) or change port according to [here](https://giscience.github.io/openrouteservice/run-instance/running-with-docker#avoid-files-owned-by-root)
