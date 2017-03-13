# PHP Docker Containers

## Overview

This is a pretty straight forward project fom running a PHP application in Docker.

## Requirements

The only requirements for running this project are Docker Engine and Docker Compose. You can find instructions
for installing these at:

- Docker Engine [https://docs.docker.com/engine/installation/]()
- Docker Compose [https://docs.docker.com/compose/install/]()

NOTE: The remainder of this document will assume you already have these installed and configured.

## Up and Running

To get up and running fast, just drop the `containers` folder and `docker-compose.yml` file into the root
of your project. Then, from the command line, run `docker-compose up -d`.

## What Happened and How Do I Use It?

After running `docker-compose`, containers for nginx, php-fpm, blackfire.io, and MySQL will build and start.
Nginx will be accessible from `http://localhost`, but default ports on the other containers are only 
exposed within the Docker network.**

Nginx is configured to send PHP requests to the PHP-FPM container. PHP-FPM has access to the MySQL 
container for queries and the host machine for sending Xdebug output. Blackfire.io profiling can be initiated 
by running the Blackfire Companion in Chrome*** while viewing the site.

** Docker exposes alternate, random ports for each of these containers to be accessed from the host machine. 
Run `docker ps` to see which ports.

*** This is my prefered method and not a requirement.

## Containers

### Nginx

The official Docker Nginx container. It has only been customized to send PHP requests to the PHP-FPM
container on port `9000`.

Version: 1.11.x

### PHP-FPM

The official Docker PHP container. This has PHP-FPM installed with extensions for MBString, Mcrypt, PDO-MySQL,
Xdebug, and Blackfire.io Agent. In order for Xdebug to send information to your development machine, you'll need
include the actual network IP address (not the internat Docker IP) of the host machine in the environment
variable `XDEBUG_HOST`. See this [https://shippingdocker.com/xdebug/auto-config/](screencast) for more details 
on how it works.

Version: 7.1.x

### Blackfire.io Agent

The official Blackfire.io Docker image. No modifications have been made to this container. Environment variables 
should be setup for `BLACKFIRE_SERVER_ID` and `BLACKFIRE_SERVER_TOKEN` on the host machine. 

Version: latest

### MySQL

The official MySQL Docker image. No modifications have been made to this container. Environment variables 
are already configured to run the image, but you may wish to make changes.

Version: 5.7.x