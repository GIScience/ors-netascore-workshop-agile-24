# Why openrouteservice? 

There are many great routing engines, here is a brief comparison.  

|                            | openrouteservice | GraphHopper  | OSRM | Valhalla |
|----------------------------|-----------------|--------------|------|---------| 
| Routing                    | ✔               | ✔            | ✔    | ✔       |
| Matrix                     | ✔               | ✔            | ✔    | ✔       |
| Isochrones                 | ✔               | ✔            |      | ✔       |
| Traveling Salesman Problem | ✔               | ✔            | ✔    | ✔       |
| Vehicle Routing Problem    | ✔               | ✔            |      | ✔         |
| Map Matching               | ✔               | ✔            | ✔    | ✔       |

[comment]: # (|||)

```{note}
This table only contains services related to navigation. Additional services are offered by the projects.
```

[comment]: # (|||)

### Routing profiles 

The routing engines offer different routing profiles by default. Additinal ones can usually be implemented, which however requires editing the source code. 


| Profiles            | openrouteservice | GraphHopper | OSRM | Valhalla                       |
|---------------------|------------------|-------------|-----|--------------------------------| 
| Pedestrian          | ✔                |  ✔          | ✔   | ✔                              |
| Hike                | ✔                |  ✔          |     |                                |
| Bike                | ✔                |  ✔          | ✔   | ✔                              |
| Mountainbike        | ✔                |  ✔          |     |                                |
| Racing bike         | ✔                |  ✔          |     |                                |
| Electric bike       | ✔                |             |     |                                |
| Car                 | ✔                |  ✔          | ✔   | ✔                              |
| Public Transport    | ✔                |             |     | ✔                              |
| Heavy Goods Vehicle | ✔                |             |     | ✔                              |
| Wheelchair          | ✔                |  ✔          |     |                                |
| Motor scooter       |                  |             |     | ✔ (beta)                       |
| Motorcycle          |                  |  ✔          |     | ✔ (beta)                       |
| Taxi                |                  |             |     | ✔                              |
| Bus                 |                  |             |     | ✔                              |
| Multimodal          |                  |             |     | ✔ (public transp. + pedestrian) |

[comment]: # (|||)


### Self hosting vs APIs 

|                         | openrouteservice | GraphHopper   | OSRM | Valhalla |
|-------------------------|------------------|---------------|------|----------|
| Self-hosting            | ✔                | ✔             | ✔    | ✔        |
| Public API              | free             | free and paid |      |          |
| Web Client - Routing    | ✔                | demo          |      | demo     |
| Web Client - Isochrones | ✔                |               |      |          |


[1] [With workaround: https://github.com/valhalla/valhalla/issues/1496](https://github.com/valhalla/valhalla/issues/1496) 
