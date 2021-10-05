# MAPTILER
###### Join local JSON data with json geometries to make choropleth map with [MAPTILER CLOUD](https://www.maptiler.com/cloud/).


In this tutorial we will show you how to make a choropleth map that plots the value of population in EU countries. 
The project will be written in Javascript using [Mapbox GL JS](https://docs.mapbox.com/mapbox-gl-js/api/) a web mapping library based on WebGL.

This example styles a choropleth map by merging local JSON data with vector tile geometries.
It uses the [match](https://docs.mapbox.com/mapbox-gl-js/style-spec/expressions/#match) expression to join the [Maptiler Countries dataset](https://cloud.maptiler.com/tiles/countries/?_gl=1*suxdhl*_ga*NjU5NTMyMDcuMTYzMzA4ODY3NQ..*_ga_K4SXYBF4HT*MTYzMzQzMTc0Mi4xLjEuMTYzMzQzMjMxMC4xNg..&_ga=2.60744238.2062873525.1633342045-65953207.1633088675) vector tileset with local JSON data by using a two-letter - [ISO alpha-2](https://www.iban.com/country-codes) country code as the lookup key for the country shape.

## What is a choropleth map?

Before we get started, let’s explain what a choropleth map is to those who might not know. A choropleth map is a map that displays quantitative data values using a range of colors. Or, as more eloquently put by [Wikipedia](https://en.wikipedia.org/wiki/Choropleth_map):

>A choropleth map (from Greek χῶρος ("area/region") + πλῆθος ("multitude")) is a thematic map in which areas are shaded or patterned proportionally to the value of a particular variable measured for each area. Most often the variable is quantitative, with a color associated with an attribute value. Though not as common, it is possible to create a choropleth map with nominal data. Choropleth maps illustrate the value of a variable across the landscape with color that changes across the landscape within a particular geographic area. [1] A choropleth map is an excellent way to visualize how a measurement varies across a geographic area.

![map](https://github.com/FridrichPeter/maptiler/blob/main/images/map.png)


## Prepare JSON data
We used a [Eurostat](https://ec.europa.eu/CensusHub2/query.do?step=selectHyperCube&qhc=false) database for this example. You simply choose what data from the census you are interested in and download it as a table. After cleaning data you have to convert CSV to JSON format easily with online [converter](https://csvjson.com/csv2json), or you can use symple csv2json python [script]


