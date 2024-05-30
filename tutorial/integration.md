# Integration

This part of the tutorial shows you how to integrate NetAScore results into openrouteservice.
For working on this tutorial, you should have successfully completed both the [NetAScore](netascore.md) and [openrouteservice](openrouteservice.md) tutorials.

## 1. Generate NetAScore results from OSM file

If you already ran NetAScore for the Glasgow OSM file we provided, you may skip this part and continue with the next section.

In order to work with consistent data, it is best to compute NetAScore using the same local OSM file as input which is also loaded into openrouteservice. 
You find the `glasgow-latest.osm.pbf` file on the [shared web drive](https://heibox.uni-heidelberg.de/d/9f1c43601ad2475f843b/).
Please download and place this file within the `data` directory you created for NetAScore. This is the directory where the settings file, mode profile files, and all output GeoPackages are located.

Then, adjust your settings file to use the local OSM file as input instead of automatically downloading data. It should contain the global `target_srid` setting, and the `filename` property within the `import` section instead of a `place_name`. 

```yaml
global:
  case_id: glasgow
  target_srid: 32630

import:
  type: osm
  on_existing: delete
  filename: glasgow-latest.osm.pbf
```

As soon as your settings file is prepared, run NetAScore as usual with `docker compose run netascore data/settings_osm_file.yml` (change the last parameter according to the name of your settings file).


## 2. Convert the results for use with openrouteservice

For use with openrouteservice, the NetAScore results need to be converted. Conversion encompasses the following aspects:

- conversion from GeoPackage format to CSV
- filtering for index values (removing additional columns)
- reducing directional index values available per road segment to a single value per OSM feature
- inverting index values (for use with openrouteservice, higher values should represent lower suitability)

All of these steps can be performed with the following Python code snippet:

```python
import sqlite3
import pandas as pd
con = sqlite3.connect("netascore_glasgow.gpkg")
df = pd.read_sql_query("""
  SELECT osm_id, 
    (1 - avg(index_bike_ft)) as index_bike,
    (1 - avg(index_walk_ft)) as index_walk
  FROM edge GROUP BY osm_id""", con)
df.to_csv("netascore_glasgow_converted.csv", index=False, na_rep="NaN", float_format="%.3f")
con.close()
```

To run this short code snippet, make sure you have the `pandas` package installed. Then either use the python interactive shell from within the NetAScore `data` directory and paste the code snippet, or place the code in a python script file at the same location and run it. Execution should be completed within seconds. Once succeeded, you should find a file named `netascore_glasgow_converted.csv` in the `data` directory.


## 3. Load the CSV as additional input to openrouteservice

To prepare for the next step, copy the CSV file `netascore_glasgow_converted.csv` to the openrouteservice `files` directory. 

With the CSV file available, you can now edit the openrouteservice configuration to include this file with the *csv storage* extension.
To do so, open the file `ors-config.yml` and adjust the following part of the profiles definition:

```yaml
      walking:
        enabled: true
        ext_storages:
          csv:
            filepath: /home/ors/files/netascore_glasgow_converted.csv
```

If you want to include the bikeability index as well, then also add the csv storage extension to the bike profile definition. 

The full description of how to set up and use the csv storage extension with openrouteservice is available [here](openrouteservice.md#let-s-do-some-walkable-routing).

Now you should be able to execute the same queries as outlined in the [Assessing Bikeability tutorial](qgis.md#assessing-bikability) with your local openrouteservice instance (make sure to edit the ORS plugin configuration accordingly).