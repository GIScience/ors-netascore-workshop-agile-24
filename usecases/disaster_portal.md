# HeiGIT Disaster Portal 

HeiGIT's disaster portal combines the openrouteservice routing and isochrones functions with crucial information about inaccessible roads during disaster events due help humanitarian organisation plan the distribution of aid. 

<img src="../img/disaster_portal.png" height="400px"></img>

Data on blocked roads due to flooding, destroyed roads or border crossing are retrieved from open sources such as the [Copernicus Emergency Management Service](https://emergency.copernicus.eu) or the humanitarian organisations themselves and stored in the backend of the disaster portal. In this way, these areas can be automatically avoided during route planning or reachability analyses. 

<img src="../img/disaster_portal_isochronen.png" height="400px"></img>


During disasters, the volunteers of the [Humanitarian OpenStreetMap Team](https://www.hotosm.org/) map the area affected by the event in OSM to support humanitarian aid organisations. To include these highly frequent changes in the OSM data, the disaster portal has its dedicated openrouteservice API which is updated every 10 minutes with the most recent OSM data.



Disaster Portal: https://disaster-portal.heigit.org/#/place/@39.46289062500001,-19.91138351415555,6
GitHub: [https://github.com/GIScience/heigit-disaster-portal[(https://github.com/GIScience/heigit-disaster-portal)

