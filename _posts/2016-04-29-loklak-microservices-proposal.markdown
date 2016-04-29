---
layout: post
title:  "Loklak apps/microservices to the open tweet platform loklak.net and IoT integrations using the loklak backend"
date:   2016-04-29 00:18:23 
categories: loklak
---

This is a publicly released version of my project proposal in Google Summer of Code 2016 to FOSSASIA for the loklak.org project. This is intended to help future applicants prepare for the proposals and also to contribute to loklak.

## Organization: FOSSASIA

### Abstract: 

loklak is free software hosted on loklak.org which provides message search results from twitter in json format. loklak is a complete search engine back-end solution with a json api. Loklak's API can be used to build twitter clones and client alike, one of them being loklak.net. One of the main features that's pending in such a tool is the ease of adding angular micro services and modularizing the existing application to run on micro services. Alongside the data store of loklak is magnificent and provides the ability to build IoT platforms using the loklak server. In this proposal I would like to enhance the service architecture of loklak.net and build a proof of concept IoT application demonstrating the capabilities of the loklak server.

### About me:

I am Sudheesh Singanamalla, a dual major student studying Computer Science & Engineering at National Institute of Technology Warangal and Technology, Entrepreneurship and Product design at Indian School of Business, Hyderabad. I am reachable at the following email IDs:

- sudheesh95@gmail.com  (Personal)
- ssudheesh@student.nitw.ac.in
- sudheesh_1543@tep.isb.edu 

My personal website is live at https://www.sudheesh.info/ and my active github profile is https://github.com/sudheesh001 

I was previously an intern with Redhat R&D Bangalore and have contributed to the development of data visualizations for their internal productivity tools and visualized the data between openshift and glusterfs. I've previously interned with Microsoft on their Azure cloud platform and built an artificial intelligent application for predicting the upcoming ticker price of stocks in the stock markets. I've worked along with Google Code for India, and worked along with an amazing team of developers to build a route optimization procedure on google maps which would be used by an NGO serving more than ten thousand children their mid day meals in the city of Bangalore and its surroundings.

My interests in computer science are web technologies, algorithms, language processors and data mining. The programming language in which I've done most of my major open source contributions are in JavaScript and Python. I generally prefer to work with JavaScript, Python, C and C++, but I am familiar with Assembly, PHP, Java and Scala. When it comes to build web applications I prefer to use Node.js, Flask (python), Django (python) and Codeigniter (PHP).




### What I would like to build in the project

I've been associated with the loklak project since the past one year and have a sound understanding of the APIs and the architecture in the project. I believe very strongly in the fact that different IoT services could use the loklak server as a data store for their applications. It could be easily set up to collect data in home intelligence IoT systems. As a part of this proposal I would like to build various IoT applications which use loklak as the main data store. At the same time there are a lot of enhancements needed in the timeline search navigation in building the loklak wall. We can have combined tweet walls which contain various micro messages from IoT platforms as well as from twitter and show them on the same loklak wall. There are a few other enhancements that are needed with the UI/UX of the application to ensure that the app performs much more better under heavier loads. As a part of making this happen, the first step is to make these services more independant and as plugins which could be directly plugged in by the user. Keeping the services independant would mean new apps could be added to the loklak platform making loklak_server the platform for various loklak apps to be built and stored as a market-place for such service integrations resulting into creation of micro services for loklak webclient. Some of these services for example include integrations of

1. Loklak search Heat maps into loklak.net as a seperate service.
2. Integrating an app marketplace option where various other apps can be easily integrated into the loklak environment.
3. Build IoT tools which use loklak_server as their main data store and provide a proof of concept on local home loklak server so that the users can maintain the power over the data that's being shared and build complete applications around that.

### Work plan

#### Pre GSoC

Add missing widgets to loklak.net like missing followers/following to /report
Enhance the reporting system and existing loklak heatmap to the new services that could be used.
Work with mentor on the data (Private/Public) issue so that the data could be pushed to the loklak backend accordingly and query the required information based on user accounts. There has to also be more work done on the user accounts, we need a way to store user information in the apps part of the loklak server where the user authentication is also a part of the apps. This is currently being done using the oauth-proxy service running as a gulp task in gulp live.
Work on the different IoT applications as a proof of concept of using the loklak server as a the main data store so that micro messages from these IoT platforms can be used on loklak and simultaneously shown on the loklak wall as well as turn up in search fields. To do this, the Tweet Response json object response needs to be extended where other than ['statuses'] array of objects we also have `['apps']` where there is a key value pair of the apps that the user is signed up to the app services/microservices, each of these apps have the structure as follows:

- {% highlight python %}
	{ 'apps': 
		{ 'app1': 
			{ 'name':'Farm Systems',
			  'messages': [
			  	{ <each message object> , …. } 
			  ],
			  'date':'...', … 
			} 
		},
		….(other apps) … 
	}
  {% endhighlight %}
- Having this ensures that we can maintain the query of all the apps and similar to how package managers behave, we can have only one name given to one micro instance at one time.
- Different requests like GET, POST, PUT, DELETE can be made to the apps so that the messages can be added to the loklak server from the IoT application by sending the timestamp and message information so that this information could also be displayed alongside the regular search queries. In such a situation where the loklak apps have some information and twitter contains the other, the UI/UX would prioritize the Loklak tweets by giving it a tag similar to “twitter sponsored” tweets/profiles and then display the other micro messages.

#### During GSoC

**Proof of concept IoT application using Loklak Server as the backend**

I would like to build the following IoT applications which use the loklak server as the backend within a home. This focuses on building tools using various sensors which take inputs and tweet about them to the local loklak server present within the home and enable multiple devices over the network to connect to each other. At the same time there’ll be a complete and a unified portal which indicates all of these devices, the messages they’ve sent to each other being IoT, instructions conveyed by the user and a proof of concept of the various details of a home automation system that the loklak server could support using micro messages. Here is a regular home scenario that I would like to implement showcasing the IoT readiness of the loklak server.

A regular home automation system for building a smart home consists of the following parameters that are directly available to the user:

1. Temperature indication and controlling.
2. Room lighting and proximities for automation light turning on / off
3. Automatically watering the plants in the farm / garden in the front yard or backyard of one’s home.
4. Automatic lighting in the house based on the time of the day, Lights need to be automatically switched on in the house and the action can be tweeted to the owner as well as send the message to the loklak server app so that the user’s home dashboard can visualize this information.

Temperature of the home can be indicated using a temperature sensor, This can be displayed on a web based dashboard for the users along with the messages that each of these devices are tweeting or sending to loklak server. The user can manually also contact the required IoT device by sending a particular instruction to it as a tweet or tweet within loklak server to the required IoT app that the server is using. These instructions sent to the loklak server can trigger the IoT platform to perform the required operation like switching on the lights in the required room or switching them off, similarly water the plants etc.., 

<img src="{{ site.baseurl }}/img/loklak-proposal-1.png" alt="Loklak home automation" width="100%">

### Brief Timeline of the tasks to accomplish

#### Pre GSoC

Week 1 - 2: Fix the existing UI/UX and app micro service based functionality in the web application with the help of mentor and add the required support on the loklak server to have an easier push API for the tweets. This includes
An easy way to send a POST request to create a loklak app

An easy way to send push object JSON to the specific app on the loklak server in a dynamic key,value based JSON response giving users the flexibility to maintain the JSON structure they’d need for big data. Bigger applications could use larger JSON responses but the aim of this enhancement would be to ensure that the JSON pushed to the server can be of dynamic nature so that the IoT applications can be implemented. API to send GET requests for the apps and receive the responses.

Week 3: Integrate tweet heat map and modularize the existing angular services such that it’s easier to add newer apps/micro services to loklak.net, Also ensure that the dynamic nature of the apps object messages as proposed above is integrated into the search and wall of the loklak.net web application

#### During GSoC

Week 1 - 2: The API built for the apps as described doesn’t have any access limitations and this week would be involving the storage of user tokens within the application as well as user account based rights to PUSH requests to the specific app on the loklak server. Enhance the integrations to the loklak server APIs to support these new app push features as described in the tracker bug which will contain the requirements for this proposal. Only a few privileged users can send push requests to the apps part, this includes the IoT application as well as the creator and a fixed other users who would be added to the same network Adding these user privileges and also mentioning the privacy type of the data i.e public / private data.

Week 3 - 4: Start building the IoT application to use loklak server as the data store so that the IoT application can tweet the required messages to the loklak server application that it has which reflects on a common dashboard that is being used within the house indicating the different messages that are being exchanged between the different IoT devices present in the home automation network. It also provides the interface where the user can type in instructions to a particular user on that network i.e. connected to the loklak server e.g. Light bulbs, temperature sensor etc., and instruct the device to do some specific action. To do this we’ll be using the Raspberry Pi or the Arduino microprocessors and loklak server as the data store.

Week 5 - 6: Build the user dashboards for the home automation system showcasing the different statuses of the devices, the status messages that they’re sending and offer analytics services on the dashboards.

Week 7 - 8: Integrate more IoT devices and connect more open data sources to the loklak data store and visualize the content over maps, dashboard containing graphs like histograms, piecharts etc..,

#### The user dashboards will look as follows

Giving the user the control to add new devices to the network to be managed by the loklak server data store

<img src="{{ site.baseurl }}/img/loklak-proposal-2.png" alt="Loklak home automation screen" width="100%">

<img src="{{ site.baseurl }}/img/loklak-proposal-3.png" alt="Loklak home automation dashboard" width="100%">

#### Message interface similar to loklak.net which will be present only on the local network.

<img src="{{ site.baseurl }}/img/loklak-proposal-4.png" alt="Loklak home automation dashboard" width="100%">

Type of data that’ll be harvested with the message harvester in loklak_server

- All the IoT nodes/devices will send micromessage to the loklak server like
	- Proximity Sensor: “It looks like there’s no one in the room as of now Maybe @lightbulb1 can turn itself off in this room”
	-Lightbulb 1: “@proximitysensor1 Yeah you’re right, it looks like there’s no one in the room, let me #initiate the turn off”
	- Turns off the light
	- Lightbulb 1: “@homenet Switched off the light in room. Lets save power”
- Data from one of the nodes can be used to build the application logic on the other, the data will be stored chronologically in order of time as a timeline and shown to the user about the different things happening in the home network and how the devices are talking to each other and the different actions they have taken throughout the day.
- The user can also specify commands to the devices and can tweet to them using loklak apps to send the micro messages. These information can be sent using the dashboard for the IoT network. The mentioned device performs the required operation using the tweet message / micromessage in the home network.

#### My previous open source contributions:

I’ve previously contributed to a lot of organizations as a contributor and actively maintain many node and python libraries which have more than 5000+ downloads each. I’ve also created the python API for loklak.org and have been mentoring the students in Google Code In 2015 with FOSSASIA to build and enhance other libraries for loklak for ruby and node.js.

I’ve been an extensive user and contributor to loklak_server and previously on the loklak webclient. I’ve also used loklak to build various other applications like GEAR which is being piloted for passive governance in the GEAR project by the Govt. Of Telangana in India and also in building telegram bots with loklak. I have more than 40+ patches to the Firefox OS and successfully submitted 99 patches to Firefox browser. I’ve also contributed in building tools like hello.js, various python libraries both scientific and nonscientific like astropy, nonude, babel and have a strong understanding of operating systems and data structures needed to build good applications. I am an active contributor on github and contribute to a lot of projects as well as maintain many of my own personal open source projects.

