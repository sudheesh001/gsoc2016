---
layout: post
title:  "Releasing the loklak python SDK 1.7"
date:   2016-08-20 00:18:23 
categories: loklak
---

Python is one of the most popular languages in which many developers from the open source community and startups write their applications, What makes this happen is the ease of usage for the developers to leverage the library. We noticed the same here at loklak, the data on the loklak server and the new integration of Susi could be leveraged with one line of code each by the developers using the library instead of writing complex reusable components to integrate loklak into their application.

<img src="{{ site.baseurl }}/img/loklak_python.png" alt="Loklak Python" width="100%">

In the v1.7 release, there have been major changes that’ve been made to the library SDK which includes direct parsing and conversion logic from one format to another i.e. `XML => JSON` / `JSON => XML` etc.., Added to this, the ability for Susi and for developers to leverage susi’s capabilities has also been integrated into the recent release. As the library matured, the library now also supports Python3 and Python2 simultaneously. It’s now very simple for a developer to leverage Susi’s capabilities because of the library.

To install the library you can do pip install python-loklak-api, works with both pip3 and pip2. Once the library is installed, it’s very simple to make queries to loklak and to susi with just a few lines of code. Here’s an example of how this could be used and the modularity and robustness with which the library has been built.

{% highlight python %}
>>> from loklak import Loklak
>>> from pprint import pprint
>>> l = Loklak() # Uses the domain loklak.org
>>> susi_result = l.susi('Hi I am Sudheesh')
>>> pprint(susi_result)
{'answer_date': '2016-08-20T04:56:17.371Z',
 'answer_time': 11,
 'answers': [{'actions': [{'expression': 'Hi sudheesh.', 'type': 'answer'}],
              'data': [{'0': 'i am sudheesh', '1': 'sudheesh'}],
              'metadata': {'count': 1, 'hits': 1, 'offset': 0}}],
 'client_id': 'aG9zdF8xODMuODMuMTIuNzY=',
 'count': 1,
 'query': 'Hi I am Sudheesh',
 'query_date': '2016-08-20T04:56:17.360Z',
 'session': {'identity': {'anonymous': True,
                          'name': '183.83.12.76',
                          'type': 'host'}}}
{% endhighlight %}

Similarly, fetching the information for a search or a user is also equally easy

{% highlight python %}
>>> l.search('rio')
>>> l.user('sudheesh001')
{% endhighlight %}

This makes it useful for hundreds of developers and plugins in Python to potentially leverage this library into various frameworks like Django, Flask, Pyramid or even run it from the command line interface. Head over to our ![github repository](https://github.com/loklak/loklak_python_api) to learn more and detailed documentation.
