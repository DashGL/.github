---
title: "Ubuntu 18.04 OSM"
description: "Notes for how to install OSM on a Ubuntu 18.04 on the Latte Panda"
pubDate: "March 19, 2019"
heroImage: "/img/Screenshot-2019-3-22-Quick-Start-Leaflet.png"
author: Kion
---

After switching from Debian to Ubuntu, the results so far have been inconclusive. I was able to install postgres, download and import the map data, make and install mapnik. And after attempting to install mod tile, I simply end up with the error:

debug: init_storage_backend: initialising file storage backend at: `/var/lib/mod_tile`

In the apache error file. However, no tiles are ever generated. Since I think most of the pieces are in place I’m going to go ahead and attempt to fiddle with what’s there and try to get something working. I can try initializing the renderd process from the command line to be able to track the output. I can try generating tiles from the command line to see if mapnik is working. And I can try to re-import the data, which I am currently running at the moment. So I think it’s a matter of testing the variables and seeing what works.

```bash
sudo apt-get install postgresql postgresql-contrib postgis postgresql-10-postgis-2.4 
postgresql-10-postgis-scripts osm2pgsql git autoconf libtool libmapnik-dev apache2-dev
```

### Debug

Okay I found the issue. By looking at ‘systemctl status renderd’ we get the following error.

Received request for map layer 'default' which failed to load

Which looks like the style.xml file doesn’t exist. So the following step is failing.

```bash
carto project.mml > style.xml
```

So the way to fix this was to install carto and then call it publicly.

```bash
sudo apt-get install npm
sudo npm install carto -g
/usr/local/lib/node_modules/carto/bin/carto project.mml > style.xml
```

Okay then we try: `http://192.168.179.21/osm_tiles/0/0/0.png`

![](/img/0.png)

### Follow Up

So here is my index.html file:

```html
<!DOCTYPE html>
<html>
<head>
	
	<title>Quick Start - Leaflet</title>

	<meta charset="utf-8" />
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	
	<link rel="shortcut icon" type="image/x-icon" href="docs/images/favicon.ico" />

    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.4.0/dist/leaflet.css" integrity="sha512-puBpdR0798OZvTTbP4A8Ix/l+A4dHDD0DGqYW6RQ+9jxkRFclaxxQb/SJAWZfWAkuyeQUytO7+7N4QKrDh+drA==" crossorigin=""/>
    <script src="https://unpkg.com/leaflet@1.4.0/dist/leaflet.js" integrity="sha512-QVftwZFqvtRNi0ZyCtsznlKSWOStnDORoefr1enyq5mVL4tmKB3S/EnC3rRJcxCPavG10IcrVGSmPh6Qw5lwrg==" crossorigin=""></script>	
</head>
<body>



<div id="mapid" style="width: 600px; height: 400px;"></div>
<script>

	var mymap = L.map('mapid').setView([35.145844, 138.681230], 2);

	L.tileLayer('/osm_tiles/{z}/{x}/{y}.png', {
		maxZoom: 18,
		attribution: 'Map data &copy; <a href="https://www.openstreetmap.org/">OpenStreetMap</a> contributors, ' +
			'<a href="https://creativecommons.org/licenses/by-sa/2.0/">CC-BY-SA</a>, ' +
			'Imagery © <a href="https://www.mapbox.com/">Mapbox</a>',
		id: 'mapbox.streets'
	}).addTo(mymap);

</script>

</body>
</html> 
```

And here’s what the result looks like:

![](/img/Screenshot-2019-3-22-Quick-Start-Leaflet.png)

I had to increase the tile missing timeout from 30 seconds to 60 seconds. The result is painfully slow, so it’s not exactly viable, but it is possible. I think the SD card is likely a huge bottleneck for the database read-write speeds. I’m going to go ahead and say that this is confirmed, you _can_ make a tile server on an SD card based ARM SoC, but you probably wouldn’t want to. Something like the Rock Pi 4 that has an nVme SSD connector would likely make a better test candidate.