---
layout: post
title:  "Architectural design for supporting susi on multiple messaging services"
date:   2016-08-11 00:18:23 
categories: loklak
---

Susi has been evolving and learning more every single day leveraging the billion+ tweets that the loklak server has indexed. The next important step would be to hookup Susi’s capabilities in a fashion that the world can easily use. A best friend powered by the data that’s scraped on every single platform available. With this in mind, we first dug deep into the facebook messenger potentially exposing susi’s capabilities to more than a billion people on the planet but as we scale and move to other agents like telegram, slack etc.., We needed some architectural changes to minimize the code duplication as well as the number of resources that we consume. In this blog post, i’ll walk you all through the design decision for the architecture planned to expose Susi to the world.

This is a detailed architecture for running all the different messaging services that we wish to accomplish in the near future. Chat / Messengers are becoming something very important and many a times the very first app that one opens up on their smart phone. It’s very important that the data in Loklak be made sense of to the people out there and learn intelligently. Susi is a great step in the process towards using the twitter data and data from other scrapers and data sources so that information can be given to people querying for it. Running a lot of services is really simple when we set up each one of the individually on a separate server but running the same code on multiple servers to just cater to one single messenger like platform ? Nah not a great idea.

Almost all of the messenger platforms be it Facebook Messenger, Telegram, Slack or anything else run on the same method, event driven and use webhooks. The idea here is to have multiple of these webhooks, and create validation endpoints for the same in case they use the GET request validations of the server like how Facebook does before verifying, At the same time many of them need SSL Certificates so that the service can be setup. This part is simplified by the heroku hosting and the default SSL that it provides for every application URL it provides.

All the services residing in the same server host/application can be used to share the common query library i.e. making the requests to /api/susi.json and returning the corresponding json or the answer entry which is available at body.answers[0].actions[0].expression , There’s a lot more information and modular architecture that can be targeted during the cleanup of each of these services into the required folders by using routing from the index.js for the same. In such a system, the index.js behaves as a proxy layer forwarding the requests to the corresponding service agent rather than scanning through the entire index.js file as it is now. So the application structure over time would look like this.

<img src="{{ site.baseurl }}/img/messenger_architecture.png" alt="Messengers Susi Architecture" width="100%">

{% highlight yml %}
|- Common\QueryBuilder.js (Common Library to be used across)
|- Facebook/
|--------\facebook.js
|--------\supportFiles.js
|- Slack/
|- Telegram/
|- Susi's Chat Interface/
|- Other Services ...,
|- index.js (Route to required agent)
{% endhighlight %}
