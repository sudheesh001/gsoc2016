---
layout: post
title:  "Dockerize the loklak server and publish docker images to IBM containers on Bluemix cloud"
date:   2016-06-15 00:18:23 
categories: loklak
---

Docker is an open source platform which makes life easy for system developers and sysadmins to build, ship and run distributed applications. Unlike Virtual Machines (VMs) Docker containers allows you to package an application with all of its dependies into a standardized unit for software development.

<img src="{{ site.baseurl }}/img/docker_container_logo.png" alt="Docker Container Logo" width="100%">

To create an image of the loklak server, the first thing you need is to run the docker daemon process. This then boots up a blank docker instance (called a `default` image) such that the images could be stored. To run loklak in a container, we first need to create a new image for loklak server. This includes the base operating system, the required updates for that operating system as well as the build instructions for running the application. To run the loklak server we need to clone the loklak_server from `loklak/loklak_server` and then build and compile using `ant`.

Once the docker daemon’s default image has booted, Create an new directory called `Docker` and `cd` into that directory. Inside that create a directory `loklak` so that you could store the `loklak` image that you would create. To do this you need to create a Dockerfile mentioning the operating system and the different instructions that need to be executed.

{% highlight dockerfile %}
FROM ubuntu:latest
MAINTAINER Ansgar Schmidt <ansgar.schmidt@gmx.net>
ENV DEBIAN_FRONTEND noninteractive

# update
RUN apt-get update
RUN apt-get upgrade -y

# add packages
RUN apt-get install -y git ant openjdk-8-jdk

# clone the github repo
RUN git clone https://github.com/loklak/loklak_server.git
WORKDIR loklak_server

# compile
RUN ant

# Expose the web interface ports
EXPOSE 80 443

# change config file
RUN sed -i.bak 's/^(port.http=).*/180/'                conf/config.properties
RUN sed -i.bak 's/^(port.https=).*/1443/'              conf/config.properties
RUN sed -i.bak 's/^(upgradeInterval=).*/186400000000/' conf/config.properties

# hack until loklak support no-daemon
RUN echo "while true; do sleep 10;done" >> bin/start.sh

# start loklak
CMD ["bin/start.sh"]
{% endhighlight %}

This fetches the latest ubuntu image and provides a non-interactive Debian front end. Docker files are executed step by step. So in the above docker file for loklak, the machine image is first pulled over and then the operting system image us updated and the distribution upgraded. Once this is done the dockerfile then runs `RUN apt-get install -y git ant openjdk-8-jdk` to install ant, git and java8 JDK. Once this is complete, the repository for the loklak server is closed and the container opens ports 80 and 443 to run it on HTTP and HTTPS port. The `sed` commands find and replace the settings for the HTTP port and HTTPS port on the configuration file of the loklak server. Once this is cloned, we run `ant` and execute `bin/start.sh` to start the loklak server.

Since now we understand how the docker file runs, lets find out how the docker is actually executed.
`docker images` to see the list of images that you have.

Now create the loklak image by doing `docker build -t loklak .` which uses the `Dockerfile` in the current directory to build the image in that directory.

The command takes several seconds to run and reports its outcome. During this process, it fetches each of the image layers and puts these image layers together to create your docker container. Once it’s installed, you can run it by doing `docker run loklak`. This has successfully created your image and it’s ready to push it to dockerhub after creating a namespace and renaming the image as `useraccountname/loklak` and then pushing to docker hub.

To push these to the IBM Bluemix cloud, you need to have `cloudfoundry` command line tools `cf`. Then install the plugin for containers `cf install-plugin https://static-ice.ng.bluemix.net/ibm-containers-linux_x64`

Once this is installed, connect to the IBM Bluemix cloud under a namespace and pull the required image you pushed to github as `cf ic cpi username/loklak loklak` which copies the image you pushed to dockerhub to the IBM Containers and run the container.

Containers make life really simple for devops and to create and deploy instantly as well as scale on demand.
