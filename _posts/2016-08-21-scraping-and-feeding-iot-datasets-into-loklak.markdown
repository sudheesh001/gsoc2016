---
layout: post
title:  "Scraping and Feeding IOT Datasets into Loklak - Part 1"
date:   2016-08-21 00:18:23 
categories: loklak
---

There’s a lot of open data that’s available online be it a government website providing opendata or different portals having a lot of information in various formats. Many data portals and IOT devices support XML, CSV and JSON type data queries and hence previously in Loklak the integration for type conversion has been made making it a very simple method call so that each source format can be converted into a destination format. This makes it really easy for other parts of the entire code to also reuse the components of the program. For example to convert from XML to JSONML, the requirement is to have a well structured XML document after which performing a method call like this below makes the conversion.

`XML.toJSONObject(xmlDataString);`

Since we now have all the required data conversion logic in place it was time to start scraping and fetching the information from various data sources that we had targeted and IOT devices. In the previous weeks, the support for tracking the GPS datasets from different GPS devices to render the location has been complete and this time we looked ahead and started with the Earthquake data sets available from the government. The data available here are classified based on duration and magnitude with the fixed values possible for each one of them as

```
duration : hour, day, week, month
magnitude: significant, 1.0, 2.5, 4.5
```

So different set of queries can be constructed which roughly translate as follows
1. Data of significant earthquakes in the last hour
2. Data of significant earthquakes in the last day
3. Data of significant earthquakes in the last week
4. Data of significant earthquakes in the last month
5. Data of earthquakes less than 1.0 richters in the last hour
6. Data of earthquakes less than 1.0 richters in the last day
7. Data of earthquakes less than 1.0 richters in the last week
8. Data of earthquakes less than 1.0 richters in the last month

Similarly other queries can also be constructed. All of this data is realtime and refreshes at 5 minute intervals enabling loklak server to harvest this data for use with Susi or providing this information to researchers and scientists / data visualization experts to perform their tasks.

Once this stream has been implemented, it was time to look at similar data structures and integrate them, most of the IOT devices sending out weather related information generally send out similar structure of information, So the next target was to integrate Yahi the haze index, these devices in singapore monitor the air quality. The data which looks like this

{% highlight json %}
[
  {
    "hardwareid": "hardwareid",
    "centerLat": "1.3132409",
    "centerLong": "103.8878271"
  },
  {
    "hardwareid": "48ff73065067555017271587",
    "centerLat": "1.348852",
    "centerLong": "103.926314"
  },
  {
    "hardwareid": "53ff6f066667574829482467",
    "centerLat": "1.3734711",
    "centerLong": "103.9950669"
  },
  {
    "hardwareid": "53ff72065075535141071387",
    "centerLat": "1.3028249",
    "centerLong": "103.762174"
  },
  {
    "hardwareid": "55ff6a065075555332151787",
    "centerLat": "1.2982054",
    "centerLong": "103.8335754"
  },
  {
    "hardwareid": "55ff6b065075555351381887",
    "centerLat": "1.296721",
    "centerLong": "103.787217",
    "lastUpdate": "2015-10-14T16:00:25.550Z"
  },
  {
    "hardwareid": "55ff6b065075555340221787",
    "centerLat": "1.3444644",
    "centerLong": "103.7046901",
    "lastUpdate": "2016-05-19T16:43:03.704Z"
  },
  {
    "hardwareid": "53ff72065075535133531587",
    "centerLat": "1.324921",
    "centerLong": "103.838749",
    "lastUpdate": "2015-11-29T01:45:44.985Z"
  },
  {
    "hardwareid": "53ff72065075535122521387",
    "centerLat": "1.317937",
    "centerLong": "103.911654",
    "lastUpdate": "2015-12-04T09:23:48.912Z"
  },
  {
    "hardwareid": "53ff75065075535117181487",
    "centerLat": "1.372952",
    "centerLong": "103.856987",
    "lastUpdate": "2015-01-22T02:06:23.470Z"
  },
  {
    "hardwareid": "55ff71065075555323451487",
    "centerLat": "1.3132409",
    "fillColor": "green",
    "centerLong": "103.8878271",
    "lastUpdate": "2016-08-21T13:39:01.047Z"
  },
  {
    "hardwareid": "53ff7b065075535156261587",
    "centerLat": "1.289199",
    "fillColor": "blue",
    "centerLong": "103.848112",
    "lastUpdate": "2016-08-21T13:39:06.981Z"
  },
  {
    "hardwareid": "55ff6c065075555332381787",
    "centerLat": "1.2854769",
    "centerLong": "103.8481097",
    "lastUpdate": "2015-03-19T02:31:18.738Z"
  },
  {
    "hardwareid": "55ff70065075555333491887",
    "centerLat": "1.308429",
    "centerLong": "103.796707",
    "lastUpdate": "2015-03-31T00:48:49.772Z"
  },
  {
    "hardwareid": "55ff6d065075555312471787",
    "centerLat": "1.4399071",
    "centerLong": "103.8030919",
    "lastUpdate": "2015-11-15T04:04:41.907Z"
  },
  {
    "hardwareid": "53ff6a065075535139311587",
    "centerLat": "1.310398",
    "fillColor": "green",
    "centerLong": "103.862517",
    "lastUpdate": "2016-08-21T13:38:56.147Z"
  }
]
{% endhighlight %}

There’s more that’s happened with IOT and interesting scenarios that happened, I’ll detail this in the next follow up blog post on IOT.
