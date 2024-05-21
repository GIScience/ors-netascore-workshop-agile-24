# How to use openrouteservice?

openrouteservice can be used via a web API. Take a look at the [openrouteservice API playground](https://openrouteservice.org/dev/#/api-docs) to find out how to query openrouteservice. 

[comment]: # (|||)

### ... via the Web Client 

To give openrouteservice a try, check out our [openrouteservice Web Client](https://maps.openrouteservice.org/#/). 


[comment]: # (|||)

### ... via the API 

If you want to integrate openrouteservice into you project, you need to use the openrouteservice API. There are two options on how to query it. 

[comment]: # (|||)

#### 1. Public API 

HeiGIT gGmbH provides a **public, global API** which is **free** to use for everyone within [certain limits depending on your plan](https://openrouteservice.org/plans/). 

1. Create an account at [https://openrouteservice.org/](https://openrouteservice.org/).
2. Generate an API key. 
3. Send queries to the public API including your API key.

[comment]: # (|||)

#### 2. Self-Hosting 

In the [openrouteservice documentation](https://giscience.github.io/openrouteservice/) you will find a detailed information on how to **set up openrouteservice** on your own machine.  

### ... via Libraries and Plugins
There are many libraries and plugins available for communicating with openrouteservice that make it easy for you to integrate calls to openrouteservice in your own code and analysis.

* JavaScript - 
* R
* Python
* QGIS - Using the ors tools plugin available from the standard QGIS plugin repo

`````{admonition} See also
:class: tip
No matter whether which option you choose, take a look at [the openrouteservice Ecosystem](./04_ecosystem.md) to learn how to send queries to the openrouteservice API in the easiest way for you. 
`````


