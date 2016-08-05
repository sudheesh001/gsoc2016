---
layout: post
title:  "Loklak now supports IOT Streams from GPS devices"
date:   2016-07-08 00:18:23 
categories: loklak
---

GPS is one of the most widely used techniques for geolocation. There are a lot of commercial GPS tracking and reporting devices that are available in the market like the Xexun 10X series, Garmin GPS Devices and lots of other open source devices like GPS Cookie and GPS Sandwich. GPS is a satellite based navigation system which accurately positions a person. These tracking devices generally have 2 functionalities STORE and STREAM.

The STORE stores the NMEA GPS data onto the SD Card / storage device present on the device. This information can later be visualized or converted into a specific format as the NMEA.txt call does on the loklak server. The STREAM service takes the data and uses a GPRS mode of communication to send the NMEA Sentences to the designated server to which it has been configured. There has to be a node listening on the port so that the devices can stream the information to that port, or it can stream to a specific URL and Loklak can use the `stream=` in its GET request to make the required conversions into visualizer data so that the NMEA Sentences can be visualized.

A sample NMEA sentence looks as follows
{% highlight js %}
/*
Sample Read Format
==================
  1      2    3        4 5         6 7     8     9      10    11
$xxxxx,123519,A,4807.038,N,01131.000,E,022.4,084.4,230394,003.1,W*6A
  |      |    |        | |         | |     |     |      |     |
  |      |    |        | |         | |     |     |      |     Variation sign
  |      |    |        | |         | |     |     |      Variation value
  |      |    |        | |         | |     |     Date DDMMYY
  |      |    |        | |         | |     COG
  |      |    |        | |         | SOG
  |      |    |        | |         Longitude Sign
  |      |    |        | Longitude Value
  |      |    |        Latitude Sign
  |      |    Latitude value
  |      Active or Void
  UTC HHMMSS
 */
{% endhighlight %}

Making a query on the given stream of such sentences on loklak returns a response as follows

{% highlight json %}
{
  "1": {
    "lat": 60.073490142822266,
    "long": 19.67548179626465,
    "time": 94154,
    "Q": 2,
    "dir": 82.0999984741211,
    "alt": 0.699999988079071,
    "vel": 1.7999999523162842
  },
  "2": {
    "lat": 60.07347106933594,
    "long": 19.675262451171875,
    "time": 94140,
    "Q": 2,
    "dir": 82.0999984741211,
    "alt": 0.10000000149011612,
    "vel": 1.7999999523162842
  }
}
{% endhighlight %}

Some devices give only a few variables in the sentence whereas some provide even information such as direction , heading and velocity. Hence there’s a need for handling multiple types of sentences in the NMEA Parsers. This is done by implementing an overloaded function parse() in the NMEAServlet which works as follows

{% highlight java %}
class GPVTG implements SentenceParser {
	public boolean parse(String [] tokens, GPSPosition position) {
		position.dir = Float.parseFloat(tokens[3]);
		return true;
	}
}
{% endhighlight %}

For a new NMEA Sentence format for example XXXXX We need to add a few lines to enhance the parser as follows by using the SentenceParser interfaces.

{% highlight java %}
class XXXXX implements SentenceParser {
	public boolean parse(String [] tokens, GPSPosition position) {
		position.dir = Float.parseFloat(tokens[X]);
		return true;
	}
}
{% endhighlight %}

This is a great milestone for loklak because it now supports IOT devices to stream to it and has the required conversion logic which converts the Sentence streams/logs into the requires JSON format so that it can be visualized. The NMEA Visualizer is coming soon where you can enter a stream URL or attach a log and it performs the required operations.

Cheers !