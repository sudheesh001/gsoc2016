---
layout: post
title:  "Developer Tools: Build Your Query Using LQL"
date:   2016-06-01 00:18:23 
categories: loklak
---
Writing request queries is definitely a hard job for developers trying to use any API. Sometimes the query strings go wrong and sometimes you're not getting the output you've been expecting. We understood a similar problem in Loklak API for developers and built the Loklak Query Language (LQL).

This tool takes in the fields and the type of query you want to make and dynamically creates the request URL in front of you. You could even test this URL and see the pretty printed JSON responses returned by the server. Here’s the best part, You get to use this with the custom URL of any loklak server that you deploy.

<img src="{{ site.baseurl }}/img/loklak-LQL.png" alt="Loklak LQL Logo" width="100%">

The team has put in quite some effort in scripting easy deployment buttons with Heroku, Bluemix, Scalingo, Docker Cloud etc.., and Tools like this help developers build the queries and look for the data they wanted.

<img src="{{ site.baseurl }}/img/loklak-LQL-1.png" alt="Loklak LQL Logo" width="100%">

There are a lot of features that are in store and shows the query for a lot of API endpoints. It’s a great tool to play with, In the future, we’d also be integrating the way the queries have to be made for different programming language APIs that we support. So that way you can directly access the required code segments directly and use them with the supporting library.

It’s a lightweight application, every time a change is made in any of the fields in the form, the query gets generated completely on the client side and at the same time the fields change based on the API call that has been chosen.

We look forward to more updates coming into this library when the IoT support for loklak rolls out. This would mean changes in the `push` API implementations and a stream based service to coordinate between devices. In the near future we'd like to move all the documentation to a read the docs sphinx implementation like other python libaries out there.

Have a look at the LQL [here](http://loklak.org/apps/LQL/) or head over to our github and give us feedback or open an [issue](https://github.com/loklak/loklak_server).
