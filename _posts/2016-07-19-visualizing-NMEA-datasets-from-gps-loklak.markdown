---
layout: post
title:  "Visualizing NMEA datasets from GPS Tracking devices with Loklak"
date:   2016-07-19 00:18:23 
categories: loklak
---

Loklak now supports the NMEA format and gives the developers access to the format in a much more friendlier JSON format which can be readily plugged in into the required map visualizer and usable on the web to create dashboards.

The stream URL is the data URL to which the GPS devices are streaming the data which need to be read by Loklak converted and then reused. For a given stream the response is as follows

{% highlight json %}
{
	"1": {
	"lat": 0,
	"long": 0,
	"time": 0,
	"Q": 0,
	"dir": 0,
	"alt": 0,
	"vel": 0
	},
	"2": {
	"lat": 0,
	"long": 0,
	"time": 0,
	"Q": 0,
	"dir": 0,
	"alt": 0,
	"vel": 0
	},
	"3": {
	"lat": 0,
	"long": 0,
	"time": 0,
	"Q": 0,
	"dir": 0,
	"alt": 0,
	"vel": 0
	},
	"4": {
	"lat": 0,
	"long": 0,
	"time": 0,
	"Q": 0,
	"dir": 0,
	"alt": 0,
	"vel": 0
	},
	"5": {
	"lat": 0,
	"long": 0,
	"time": 0,
	"Q": 0,
	"dir": 0,
	"alt": 0,
	"vel": 0
	},
	"6": {
	"lat": 0,
	"long": 0,
	"time": 0,
	"Q": 0,
	"dir": 0,
	"alt": 0,
	"vel": 0
	}
}
{% endhighlight %}

We now need to visualize this information. Loklak now has a built in tool which can make this happen present here. You’ll see a screen which asks for the stream URL, Provide the URL where the GPS device is sending the information

<img src="{{ site.baseurl }}/img/NMEA_1.png" alt="nmea load data" width="100%">

This visualizes the information

<img src="{{ site.baseurl }}/img/NMEA_2.png" alt="nmea display data" width="100%">

This is possible with the data format the `NMEA.txt` servlet is performing and the client side packaging of the objects while loading them onto a map.

{% highlight js %}
function getTracking() {
	var url = document.getElementById('url').value;
	var cUrl = window.location.href;
	var pName = window.location.pathname;
	var baseUrl = cUrl.split(pName)[0]
	var urlComplete = baseUrl+'/api/nmea.txt?stream='+url;
	var centerlat = 52;
	var centerlon = 0;
	// set default zoom level
	var zoomLevel = 2;
	$.getJSON(urlComplete, function (data) {
		var GPSObjects = [];
		for (var key in data) {
			var obj = data[key];
			var latitudeObject, longitudeObject
			for (var prop in obj) {
				if (prop == 'lat') {
					latitudeObject = obj[prop];
				}
				if (prop == 'long') {
					longitudeObject = obj[prop];
				}
			}
			var marker = L.marker([latitudeObject, longitudeObject]).addTo(map);
			spiralCoords = connectTheDots(marker);
			var spiralLine = L.polyline(spiralCoords).addTo(map)
		}
	});
}
{% endhighlight %}

A request is made to the required NMEA data stream and stored into the required latitude and longitude which is then each individually pushed onto the map.

Loklak is now slowly moving towards supporting multiple devices and visualizing the data that it obtains from the device data streams. The NMEA is a global standard for GPS and any tracking devices therefore loklak actively supports more than 10 devices Garmin 15, 15H etc.., from the Garmin series, Xexun 10X series and the Navbie series.

