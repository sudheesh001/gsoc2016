---
layout: post
title:  "Automated deployments and developmental pipeline for loklak server"
date:   2016-07-26 00:18:23 
categories: loklak
---

Loklak Server project has been growing and seeing an increase in the users consuming the APIs, the contributors and has been extending its reach into other territories like IOT and Artificial Intelligence chats. As the project grew, It became important to keep the server easily deployable which was done previously by integrating the one click button deployment procedures to Loklak Server for anybody to spin up their own servers.

As we grew we made quite a few mistakes in the development, overriding others’ work, conflicting patches and the system kept breaking as we migrated from Java 7 to Java 8. To avoid problems due to an increase in contributions and a lot of members working together, there needed to be a stronger engineering workflow to ensure that the development still goes unhampered and there’s lesser time taken to pull and review.

<img src="{{ site.baseurl }}/img/travis_bridge.jpg" alt="Travis Bridge Deployment Pattern" width="100%">

We’ve strongly adopted the build and release per commit where instead of periodically taking the upstream changes and deploying onto the server we now leverage existing continuous integration tools that we’ve employed to run the builds for us to also perform the deployments onto the staging / dev servers. This was done using Heroku and Travis, where every successful travis build runs a trigger to Heroku to deploy and run the server on the staging server. This has dramatically reduced the errors that we encountered before and also proved as the testing ground for new features before moving them to the production server at loklak.org

Implementation

{% highlight yml %}
deploy:
provider: heroku
email: robert.mader@posteo.de
strategy: git
buildpack: https://github.com/loklak/heroku_buildpack_ant_loklak
api_key:
secure: D2o+G28w42F9rDbde......PL/Q=
app: loklak-server-dev
on:
branch: development
{% endhighlight %}

Cheers !