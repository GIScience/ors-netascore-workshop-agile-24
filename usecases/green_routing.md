# Healthy Routing

For pedestrians and cyclists, the fastest route is not always the best, since factors such as traffic, road infrastructure or their surroundsings play a much bigger role than for car driver. Therefore, we are researching and developing specialised routing profiles which give better and healthier route suggestions especially for vulnerable groups such as the elderly or children. 

## Green and noise avoiding routing 

The green routing suggests routes which preferably lead alongside vegetation such as parks or residential gardens, while keeping the detour to a minimum. The same has been done to avoid loud streets due to high traffic. Both have been integrated within the [meinGrün App](https://meingruen.org/).

<img src="../img/meingruen.png" width="300px"></img>

To get a green instead of the fastest route, a green index is derived for each street segment. This index is combined with the length of the road to adjust the cost function using the routing algorithm to find the best route. 

<img src="../img/green_index.jpg" width="500px"></img>

## Heatstress avoiding routing 

Due to climate change, heat stress is and will continue to be a growing problem in cities. In the [HEAL project](https://www.geog.uni-heidelberg.de/gis/heal_en.html), we are developing a routing service which suggests routes for pedestrians which avoids places with large heat stress based on real-time sensor measurements in the city. The first prototype has been implemented within the [meinGrün App](https://meingruen.org/).  

<img src="../img/heatstress_routing.png" width="500px"></img>

**References:**

- Novack, T., Wang, Z., & Zipf, A. (2018). A system for generating customized pleasant pedestrian routes based on OpenStreetMap data. Sensors, 18(11), 3794.
- Wang, Z., Novack, T., Yan, Y., & Zipf, A. (2020). Quiet route planning for pedestrians in traffic noise polluted environments. IEEE Transactions on Intelligent Transportation Systems, 22(12), 7573-7584.
- Ludwig, C., Lautenbach, S., Schömann, E. M., & Zipf, A. (2021). Comparison of Simulated Fast and Green Routes for Cyclists and Pedestrians. In 11th International Conference on Geographic Information Science (GIScience 2021)-Part II. Schloss Dagstuhl-Leibniz-Zentrum für Informatik.
- Hecht, R., Artmann, M., Brzoska, P., Burghardt, D., Cakir, S., Dunkel, A., ... & Zipf, A. (2021). A web app to generate and disseminate new knowledge on urban green space qualities and their accessibility. ISPRS Annals of the Photogrammetry, Remote Sensing and Spatial Information Sciences, 8, 65-72.