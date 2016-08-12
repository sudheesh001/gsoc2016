---
layout: post
title:  "Setting up Susi's capabilities on Facebook messenger"
date:   2016-08-08 00:18:23 
categories: loklak
---

Facebook’s messenger platform is a great way to reach out to a lot of people from a page that one owns on facebook. The messenger services reach out to almost 900 million people who use the system, that’s a humongous set of people to which Susi’s capabilities could be reached out to if integrated with the facebook messenger and that’s exactly what has been done. Susi is the AI System running in Loklak which contains rules to run the required scrapers or fetch the information directly from facebook. To set this up, we created a new repository deployed on heroku called asksusi_messengers which is going to be a collection of such messenger integrations i.e. to Facebook, Slack, Whatsapp, Telegram etc.., So that the power of mobile and messaging services can be used to make Susi smarter and ways in which people can consume Susi’s capabilities.

Considering the real time nature of people and messages, we have used node.js to take requests by using a webhook on facebook which subscribes to the changes or any event that triggers from the asksusisu facebook page. So here’s the way they really work. Messenger bots uses a web server to process messages it receives or to figure out what messages to send. You also need to have the bot be authenticated to speak with the web server and the bot approved by Facebook to speak with the public. This means that we create the messenger service with facebook apps and then register the endpoint so that facebook can trigger that URL endpoint more like a webhook so that the messages that were sent by the user can be sent to Susi’s AI Service.

Using express this is pretty simple and can be accomplished by the following piece of code that listens to the root of the application.
{% highlight js %}
app.get('/', function (req, res) {
res.send('Susi says Hello.');
});
{% endhighlight %}

This ensures that the application is active for a user trying to hit the GET endpoint of the service and as a security method, the rest of the application endpoints need to be on a POST endpoints so that the application’s responses are secure and not anyone can send requests without the required tokens.

Facebook needs an SSL based hostname so that the service can be binded for this. The fastest way this can be spun up is by using the heroku deployments, Hence we use a ProcFile with the contents in it as

{% highlight js %}
web: node index.js
{% endhighlight %}

Then configure the application on facebook developers with the given name and setup a messenger webhook over there to send an event to the heroku server that you just deployed, Added to this add your own token which you need to remember and put up on heroku too. After this you will receive a page access token which you can temporarily save somewhere and later push to heroku as a configuration. You can read this configuration by doing and facebook’s verification works.

{% highlight js %}
var token = process.env.FB_PAGE_ACCESS_TOKEN;

// for facebook verification
app.get('/webhook/', function (req, res) {
if (req.query['hub.verify_token'] === 'this_is_my_top_secret_token_that_i_know') {
res.send(req.query['hub.challenge']);
}
res.send('Error, wrong token');
});

You can then receive the information from facebook’s events as follows on a POST request endpoint to the webhook

// to post data
app.post('/webhook/', function (req, res) {
var messaging_events = req.body.entry[0].messaging;
}
{% endhighlight %}

The messaging_events gets the required information from facebook in
`event.message` and `event.message.text` objects of the triggered webhook event. The query to susi is then constructed

{% highlight js %}
// Construct the query for susi
var queryUrl = 'http://loklak.org/api/susi.json?q='+encodeURI(text);
var message = '';
// Wait until done and reply
request({
url: queryUrl,
json: true
}, function (error, response, body) {
if (!error && response.statusCode === 200) {
message = body.answers[0].actions[0].expression;
sendTextMessage(sender, message);
} else {
message = 'Oops, Looks like Susi is taking a break, She will be back soon';
sendTextMessage(sender, message);
}
});
{% endhighlight %}

And voila we have the susi facebook page automatically replying powered by Loklak’s capabilities of Susi. Currently susi can reply with the text messages as well as image responses.

<img src="{{ site.baseurl }}/img/facebook_messenger.png" alt="Facebook Susi Integration" width="100%">
