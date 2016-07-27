---
layout: post
title:  "IOT data push in different formats now supported"
date:   2016-06-06 00:18:23 
categories: loklak
---

Lots of IOT devices, that are out there currently log a lot of information as CSV files. At the same time there’s a lot of data generated to data stream services from devices and satellites alike that are present in XML format. A lot of the structured storage data is exported by developers into formats like XML on most IOT devices and hence these dashboards contain options like export to XML/CSV/JSON formats.

The loklak server can store JSON formats but since the devices can send out any type of information, it becomes important for the server to support multiple data formats for both the streams as well as device pushes to the server. Over the last week we’ve had integrations to parse XML contents and convert these to JSON and publically expose them as an API endpoint available at /api/xml2json.json and data being the GET/POST parameter that needs to be sent out. This allows a lot of clients to be built around this which convert XML to JSON. We’ll soon have an app to show how this is possible and integrations into the LQL application which is an application meant for making loklak API easier for developers who are trying to use and access loklak data and the server.

Similarly the support for CSV to JSON formats is also now supported. A major problem that we hit here while development is the conversion schemes, the way special characters like carriage returns, new lines etc.., r, n etc.., are parsed and converted, Directly passing these in the parameter data string makes it hard to replace and takes up valuable string searching and processing. To overcome this, the client itself encodes the URI fields r as %0A%0D  and n as %0A accordingly and pass the data strings to the publically exposed /api/csv2json.json and data being it’s GET/POST parameters with CSV encoded information. Every CSV sent is converted and returned as a JSON which can be caught by the corresponding .success() equivalent functions of the programming language being used by the developer of the IOT device / service owner and retrigger a push response to the loklak server making this data harvesting as a completely synchronous and step by step process because of data format conversions.

This data API is publicly accessible which means it can potentially support a lot of applications that are currently trying out to use their own lexical parsers and tokenizers for making the data conversions.

For eg.

{% highlight csv %}
twittername, name, org
0rb1t3r, Michael Peter Christen, FOSSASIA
sudheesh001, Sudheesh Singanamalla, FOSSASIA
{% endhighlight %}

in the URL
<pre>
http://localhost:9000/api/csv2json.json?data=twittername,name,org%0A0rb1t3r,Michael%20Peter%20Christen,FOSSASIA%0Asudheesh001,Sudheesh%20Singanamalla,FOSSASIA%0A
</pre>

Resulting in the json format as follows
{% highlight json %}
[
  {
    "org": "FOSSASIA",
    "name": "Michael Peter Christen",
    "twittername": "0rb1t3r"
  },
  {
    "org": "FOSSASIA",
    "name": "Sudheesh Singanamalla",
    "twittername": "sudheesh001"
  }
]
{% endhighlight %}

The CDL.java, XML.java files contain the required helper classes and methods needed for the required operations, this makes string conversions anywhere in loklak server if needed as simple as String jsonData = XML.toJSONObject(data).toString(); and JSONArray array = CDL.toJSONArray(json); for the corresponding XML and CSV conversions respectively.

The Tokeners, parse and pick out tokens matching

{% highlight java %}
static {
       entity = new java.util.HashMap(8);
       entity.put("amp",  XML.AMP);
       entity.put("apos", XML.APOS);
       entity.put("gt",   XML.GT);
       entity.put("lt",   XML.LT);
       entity.put("quot", XML.QUOT);
   }
{% endhighlight %}

The XML parser looks for the starting angle brackets `<` and ending angle brackets `>` and parses the content in between them finding out the contents in between the XML tags, Once this information is obtained as the key value, it looks for the format of `>...</` and retrieves the value of the given XML tag. It gets pushed into a JSON object and forms a `key:value` pair. A collection of such objects are created for every complete XML structure given the DTD Schema.

These additions prove very useful to the IOT devices and services whose data is going to be integrated into the loklak server. The next task is to take up the Home automation system from IBM Watson cloud and make it parallely push the data to loklak now that we support so many data formats. We are very close to supporting real time applications streaming data to the loklak server.
