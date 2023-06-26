# HeiGIT Disaster Portal 

HeiGIT's disaster portal combines the openrouteservice routing and isochrones functions with crucial information about inaccessible roads during disaster events due help humanitarian organisation plan the distribution of aid. 


<!--One notable use case of openrouteservice is its ability to incorporate polygon geometries using the avoid_area option. This feature allows users to specify areas to be avoided during routing or isochrone calculations. While the primary application has been in disaster-related scenarios, such as avoiding affected areas, this functionality can also be employed for various purposes, such as scheduling routes to bypass no-go zones. A recent development building on top of the feature is the disaster portal, a web platform that enables organizations to centrally manage the areas to be avoided. Humanitarian organizations responding to the Syria-Turkey crisis, for instance, utilized this portal to mark and maintain closed borders and roads. This enabled the retrieval of alternative routes and supported on-site navigation and logistics. -->

<img src="../img/disaster_portal.png" height="400px"></img>

Data on blocked roads due to flooding, destroyed roads or border crossing are retrieved from open sources such as the [Copernicus Emergency Management Service](https://emergency.copernicus.eu) or the humanitarian organisations themselves and stored in the backend of the disaster portal. In this way, these areas can be automatically avoided during route planning or reachability analyses. 

<img src="../img/avoidance_example.png" width="600px"></img>


During disasters, the volunteers of the [Humanitarian OpenStreetMap Team](https://www.hotosm.org/) map the area affected by the event in OSM to support humanitarian aid organisations. To include these highly frequent changes in the OSM data, the disaster portal has its dedicated openrouteservice API which is updated every 10 minutes with the most recent OSM data.



Disaster Portal: https://disaster-portal.heigit.org/#/place/@39.46289062500001,-19.91138351415555,6
GitHub: [https://github.com/GIScience/heigit-disaster-portal[(https://github.com/GIScience/heigit-disaster-portal)

