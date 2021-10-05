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
We used a [Eurostat](https://ec.europa.eu/CensusHub2/query.do?step=selectHyperCube&qhc=false) database for this example. You can simply choose what data from the census you are interested in and download it as a table. After cleaning data you have to convert CSV to JSON format easily with online [converter](https://csvjson.com/csv2json), or you can use symple csv2json python [script](https://github.com/FridrichPeter/maptiler/blob/main/csv2json.py).

## Make a simple WEBMAP
As we mentioned above, for this project we used a Mapbox GL JS library, that loads vector tiles and style from hosting and draws a map in the browser. This solution requires WebGL support in a browser. See also MapBox GL JS examples and the full [API Specification](https://docs.mapbox.com/mapbox-gl-js/api/). Just copy the code below and start with your web app:

```
<!DOCTYPE html>
<html>
<head>
  <meta name="viewport" content="initial-scale=1,maximum-scale=1,user-scalable=no" />
  <script src="https://cdn.maptiler.com/mapbox-gl-js/v1.5.1/mapbox-gl.js"></script>
  <link href="https://cdn.maptiler.com/mapbox-gl-js/v1.5.1/mapbox-gl.css" rel="stylesheet" />
  <link href="/static/css/cloud_base.css?t=1633415986" rel="stylesheet" />
  <style>
    #map {position: absolute; top: 0; right: 0; bottom: 0; left: 0;}
  </style>
</head>
<body>
  <div id="map">
    <a href="https://www.maptiler.com" style="position:absolute;left:10px;bottom:10px;z-index:999;"><img src="https://api.maptiler.com/resources/logo.svg" alt="MapTiler logo"></a>
  </div>
  <p><a href="https://www.maptiler.com/copyright/" target="_blank">© MapTiler</a> <a href="https://www.openstreetmap.org/copyright" target="_blank">© OpenStreetMap contributors</a></p>
  <script>
    // You can remove the following line if you don't need support for RTL (right-to-left) labels:
    mapboxgl.setRTLTextPlugin('https://cdn.maptiler.com/mapbox-gl-js/plugins/mapbox-gl-rtl-text/v0.1.2/mapbox-gl-rtl-text.js');
    var map = new mapboxgl.Map({
      container: 'map',
      style: 'https://api.maptiler.com/maps/hybrid/style.json?key=vPjhHhi7BLLsqlePzTFr',
      center: [0, 0],
      zoom: 1
    });
  </script>
</body>
</html>
```
### Store JSON data from cenzus in to variable

```
    const data = [
    {
      'No': 1,
      'country': 'BE',
      'pop': 11522440
    },
    {
      "No": 2,
      "country": "BG",
      "pop": 6951482
    }, ...
```
### Add source for country polygons using the Maptiler Countries tileset
```
map.addSource('countries', {
          type: 'json',
          url: 'https://api.maptiler.com/tiles/countries/tiles.json?key=vPjhHhi7BLLsqlePzTFr'
        });
```
Add source for country polygons using the [Maptiler Countries dataset](https://docs.maptiler.com/schema/countries/#administrative). The polygons contain an ISO alpha-2 code which can be used to for joining the data.

