# README

This is an approach to have an image/container with mosquitto running on Ubuntu 16.04 and understand the differences between this installation and the installation made for CBF2.

## Docker Dilemas

Docker containers create from the image were exiting afer running the script to start mosquitto with the changed .conf file. So, we end up with nothing running. This [blog post](http://phusion.github.io/baseimage-docker/) highlighted the issue (although is not complety correct according to the blog post).

This sentence from the blog post pointed to the correct path:

>Here's how the Unix process model works. When a system is started, the first process in the system is called the init process, with PID 1. The system halts when this processs halts. If you call CMD ["/my_app/start.sh"] in your Dockerfile, then start.sh is your init process.

The result is that we need to launch mosquitto through CMD docker command.

The other issues was related to the fact that mosquitto user needs to have access to the /mqtt folder that we have created and where we have put the mosquitto.conf and we are putting the logs - mosquitto uses the mosquitto os user to control and have access to all the elements.

## Docker Commands

* create image - _docker build -t ub-mosquitto ._
* create and run container for the first time - _docker run -it -p 1883:1883 --name ub1604-mosquitto ub-mosquitto_
* start an existing container (this is going to start mosquitto and make it available) - _docker start ub1604-mosquitto_
* stop a running container - _docker stop ub1604-mosquitto_
* attach to the running container - _docker exec -it ub1604-mosquitto bash_

## Mosquitto commands

Using this container makes mosquitto available only for user mosquitto with password 1234. This is controlled by configuration files and password key files.

* publish to topic _/test/data/_ - _mosquitto\_sub -h localhost -t "/test/data/" -u "mosquitto" -P "1234"_
* subscribe topic _/test/data/_ - mosquitto_sub -h localhost -t "/test/data/" -u "mosquitto" -P "1234"
