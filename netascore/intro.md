![NetAScore Logo](../img/netascore/netascore_logo.png)

## What is NetAScore?
NetAScore provides a toolset and automated workflow for computing bikeability, walkability and related indicators from publicly available network data sets. Currently, we provide common presets for assessing infrastructure suitability for cycling (_bikeability_) and walking (_walkability_). By editing settings files and mode profiles, additional modes or custom preferences can easily be modeled.

For global coverage, we support *OpenStreetMap* data as input. Additionally, Austrian authoritative data, the *'GIP'*, can be used if you work on an area of interest within Austria.

NetAScore ...

- ... is a (commandline) software tool for assessing infrastructure suitability for different modes
- ... provides default profiles for cycling and walking
- ... can be used as foundation for advanced network analyses e.g. in urban planning
- ... supports OpenStreetMap and GIP (for Austria) network data
- ... is open source

### Example of segment-based bikeability for Salzburg, Austria

![NetAScore bikeability example for Salzburg, Austria](https://user-images.githubusercontent.com/24413180/229191339-7271e4ac-5a9b-4c12-ad02-dd3909215623.png)


## How to use NetAScore
If you want to run NetAScore you can choose between two modes, you can either use our ready-to-use setup that utilizes a docker image or you can run it manually using Python.

### ... using Docker
NetAScore comes with a `docker compose` configuration in `docker-compose.yml` and a demo configuration file, so you can simply run an example workflow by following the tutorial presented in the [guide on our GitHub-page](https://github.com/plus-mobilitylab/netascore/wiki/How-to-run-the-project-in-a-Docker-environment). The great advantage of running NetAScore in Docker is that you do not have to install any software or python dependencies on your machine.

### ... on your local machine using Python

If you prefer to execute NetAScore directly on your machine, virtual environments or Conda may help with managing all Python requirements. You can learn how to run NetAscore manually on your local machine by reading the [guide on our GitHub-page](https://github.com/plus-mobilitylab/netascore/wiki/Run-NetAScore-manually-with-Python).
