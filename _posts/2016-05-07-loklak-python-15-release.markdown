---
layout: post
title:  "Loklak Python API Release v1.5 and Changelog"
date:   2016-05-05 00:18:23 
categories: loklak
---
The Python API for loklak has made it really simple for using loklak in many applications which are twitter based and want to move towards Loklak. This API has also been a great point for a lot of researchers working on twitter data analysis and other sentiment analysis tools based on twitter data. The API needed quite a few enhancements the most important being the support for private loklak servers instead of the only support for `loklak.org`.

In this update, the team has made some amazing changes to the API and some of them are as follows:

- Support for local/private loklak servers and multiple loklak server objects.
- Reduced computation and lighter library
- Image responses and support for getMarkdown and getMap functionalities with text overlay over them.
- Unit tested completely
- Added Examples for each of the ways to use the python API for loklak
- Fallback mechanisms and failures on private servers moves to production `loklak.org` server.
- Command line interface for loklak using the `loklak` command on the commandline , allowing shell scripts and other devops tools to take advantage.
- Integrated the GeoCode API support in python.
- Supports unicode and doesn't break on encountering an unicode character.

This can now be easily installed from [pypi](https://pypi.python.org/pypi/python-loklak-api/) by doing `pip install python-loklak-api`

<div class="row">
	<div class="col-md-10 col-md-offset-1">
		<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Just released the v1.5 API for <a href="https://twitter.com/hashtag/loklak?src=hash">#loklak</a> in python at <a href="https://t.co/7mPssQ9bRW">https://t.co/7mPssQ9bRW</a> Now makes it possible to query public/private loklak server <a href="https://twitter.com/hashtag/IOT?src=hash">#IOT</a></p>&mdash; SudheeshSinganamalla (@sudheesh001) <a href="https://twitter.com/sudheesh001/status/724306100552503296">April 24, 2016</a></blockquote>
		<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>
	</div>
</div>

We look forward to more updates coming into this library when the IoT support for loklak rolls out. This would mean changes in the `push` API implementations and a stream based service to coordinate between devices. In the near future we'd like to move all the documentation to a read the docs sphinx implementation like other python libaries out there.
