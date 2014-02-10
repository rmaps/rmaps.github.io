---
title: Leaflet Heat Maps
author: Ramnath Vaidyanathan
class: dark
framework: thinny
highlighter: prettify
hitheme: twitter-bootstrap
mode: selfcontained
url: {lib: ../../libraries}
image: crime_map.png
date: "2014-02-08"
description: >
  This is a short post on how to use rMaps, Leaflet and the Leaflet Heatmap plugin to visualize spatial distribution of crime.
---




## Create Map

We start by creating a map of the location.


```r
library(rMaps)
L2 <- Leaflet$new()
L2$setView(c(29.7632836,  -95.3632715), 10)
L2$tileLayer(provider = "MapQuestOpen.OSM")
L2
```

<iframe src='
fig/leaflet_map.html
' scrolling='no' seamless
class='rChart leaflet '
id=iframe-
chart9a9f26605a4
></iframe>
<style>iframe.rChart{ width: 100%; height: 400px;}</style>



## Get Data

We will use the `crime` dataset from the `ggmap` package that contains a tidied up version of Houston crime data from January 2010 to August 2010. 


```r
data(crime, package = 'ggmap')
library(plyr)
crime_dat = ddply(crime, .(lat, lon), summarise, count = length(address))
crime_dat = toJSONArray2(na.omit(crime_dat), json = F, names = F)
cat(rjson::toJSON(crime_dat[1:2]))
```

```
[[27.5071143,-99.5055471,1],[29.4836146,-95.0618715,10]]
```


## Add HeatMap

Now that we have the map and the data, the next step is to add the data to the map as a heatmap layer. Thanks to the [Leaflet.heat](https://github.com/Leaflet/Leaflet.heat) plugin written by the Vladimir Agafonkin, the author of LeafletJS, this is really easy to do, with a little bit of custom javascript.


```r
# Add leaflet-heat plugin. Thanks to Vladimir Agafonkin
L2$addAssets(jshead = c(
  "http://leaflet.github.io/Leaflet.heat/dist/leaflet-heat.js"
))

# Add javascript to modify underlying chart
L2$setTemplate(afterScript = sprintf("
<script>
  var addressPoints = %s
  var heat = L.heatLayer(addressPoints).addTo(map)           
</script>
", rjson::toJSON(crime_dat)
))

L2
```

<iframe src='
fig/leaflet_heatmap.html
' scrolling='no' seamless
class='rChart leaflet '
id=iframe-
chart9a9f26605a4
></iframe>
<style>iframe.rChart{ width: 100%; height: 400px;}</style>


Note that it is easy to abstract away these additional steps into a method for the Leaflet class, or an R function that only needs to be provided the data.


<style>
  table.nofluid {width: auto; margin: 0 auto;}
  pre {margin-left: 0px;}
</style>




