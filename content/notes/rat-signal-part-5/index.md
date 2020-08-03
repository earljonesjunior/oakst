---
title: "Rat Signal Part 5: Deployment"
date: 2020-08-02T14:00:45-05:00
slug: "rat-signal-part-5"
draft: false
tags: ["docker", "mqtt"]
project: "rat-signal"
description: ""
---

[Docker](https://www.docker.com/) makes it insanely easy to deploy microservice-based applications.
Docker is used to create "containerized" services. These containers are similar to traditional
virtual machines but are more lightweight and easier to deploy and network together.

## Dockerfiles

A Dockerfile is a set of instructions used to deploy a Docker container. This is the Dockerfile
used in the Endpoint service:

```dockerfile
FROM node:12.18.3-alpine

WORKDIR /usr/src/app

COPY package*.json ./

RUN npm install

COPY . .

EXPOSE 8080

CMD npm run dev:start
```
This tells Docker to base the new image off of an image of Alpine Linux with Node and NPM preinstalled.
A working directory is set, `package.json` is copied over, the node dependencies are installed, the source
code is copied over, and the code is built. Once deployed to a container, the server is automatically run.

## Docker Compose

Docker Compose is used to coordinate deployment of all of the services used in a microservice-based 
application. It's an absolutely beautiful and elegant symphony of tasks. This is Rat Signal's Compose file:

```yaml
version: '3.8'

services:

  broker:
    image: eclipse-mosquitto:1.6.10
    restart: on-failure
    ports:
      - "1883:1883"
      - "9001:9001"
    networks:
      rat-net:
        aliases:
          - broker
    volumes:
      - "./broker/mosquitto.conf:/mosquitto/config/mosquitto.conf"

  middleman:
    image: nodered/node-red:1.1.2
    restart: on-failure
    ports:
      - "1880:1880"
    networks:
      rat-net:
        aliases:
          - middleman
    volumes:
      - "./middleman:/data"

  endpoint:
    build: ./endpoint
    ports: 
        - "8080:8080"
    networks:
      rat-net:
        aliases:
          - endpoint

networks:
  rat-net:
```

## Home Server Deployment

All of the code for Rat Signal lives in my [Github repository](https://github.com/bruthaearl/rat-signal).
To deploy Rat Signal to my home server, all I have to is SSH into the server, clone the code, and run
`docker-compose up`. Docker handles all of the heavy lifting. That's it!

The back-end server software is now complete. Next up, the front-end hardware.

---

# The Rat Signal Series:

- [Rat Signal Part 1: Overview](/notes/rat-signal-part-1)
- [Rat Signal Part 2: The Broker](/notes/rat-signal-part-2)
- [Rat Signal Part 3: The Endpoint](/notes/rat-signal-part-3)
- [Rat Signal Part 4: The Middleman](/notes/rat-signal-part-4)
- *[Rat Signal Part 5: Deployment](/notes/rat-signal-part-5)*
- [Rat Signal Part 6: Red Alert](/notes/rat-signal-part-6)
- Rat Signal Part 7: Numbers are Nice (TBA)
- Rat Signal Part 8: The OK Wave off Kanagawa (TBA)