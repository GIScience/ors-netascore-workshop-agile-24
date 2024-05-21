# Introduction

The goal of this tutorial is to introduce you to some technologies

There are four parts of the tutorial which can be worked on (mostly) in any order. If you want to focus on one in 
particular, then that's fine.

The sections of the tutorial are as follows:

## Installing and running a local instance of NetAScore
In this part of the tutorial, you will be guided through the process of running NetAScore on your own machine. By 
the end of it, you will have got an instance up and running, and used it to create bikability and walkability 
information for Glasgow. For those wanting to get more in depth, you will also be instructed as to how to do the 
same for a region that you define yourself, either by name or through providing your own OSM pbf file.

## Installing and running a local instance of openrouteservice
For this part of the tutorial, the goal is to have your very own instance of openrouteservice running on your 
machine. You will be instructed on how to get the instance running, where different things are located in the 
folder structure, and how to change configurations to be have different features running. By the end of the section, 
you will be able to query this local instance and get results for different routing tasks such as reachability and 
directions.

## Interpreting bikeability and walkability in QGIS
Using the openrouteservice QGIS plugin and a special public openrouteservice instance, this section will show you 
how you can include openrouteservice results in QGIS, in particular routes that take into account bikability and 
walkability. Using these results, you will be able to make assessments as to whether specific areas (e.g. a city 
district) are walkable or not.

## Integrating results from NetaScore into openrouteservice
This part of the tutorial is for those people who have managed to get openrouteservice and NetAScore running on 
there local machines, and would like to bring the two together. You will be guided through the process of 
downloading OSM data and then putting this into both NeAScore and openrouteservice. You will then be guided through 
the process of converting data obtained from NetAScore into a format compatible with openrouteservice, before 
integrating this data into openrouteservice so that you can do your own routing processes that take into account 
walkability.