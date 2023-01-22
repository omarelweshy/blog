---
date: 2022-12-17 11:14:00 GMT+2
categories: [Backend, DevOps]
tags: [Nginx, Docker]
comments: true
image:
  src: /media/nginx-docker.png
  width: 800
  height: 533
  alt: Nginx Docker Cover
---

## Introduction

In today's fast-paced digital world, it's crucial to ensure that your web applications are always available and can handle high traffic. That's where a load balancer comes in - it distributes incoming traffic evenly across multiple servers, maximizing performance and minimizing downtime.

But setting up a load balancer can be a daunting task, especially for those who are new to the world of web development. That's why we've created this guide - to walk you through the process of creating a load balancer using Docker and NGINX. These tools are powerful, flexible and easy to use, making them the perfect choice for load balancing.

## Preparation

You need to install the following:

- Nodejs
- Docker
- Nignx

## File Structure

Our file Structure will be like this

![Docker Node Server](/media/node-docker.png)

Dockerfile

```docker
FROM node:16
WORKDIR /home/node/app
COPY app /home/node/app
RUN npm install
RUN npm run app
EXPOSE 3000
```

index.js

```js
const app = require("express")();

app.get("/", (req, res) => {
  res.send(`Hello from ${process.env.appId} docker`);
});

app.listen(3000, () => console.log("app at http://localhost:3000"));
```

package.json

```json
{
  "name": "app",
  "main": "index.js",
  "scripts": {
    "app": "node index.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

## Create Multiple Container

We need to create multiple images point to the nodejs server with different ports

First you should build the app into Container

```bash
docker build -t nodeapplication .
```

then

```bash
docker run -p <PORT>:3000 -e appId=<ID> -d nodeapplication
```

the result

![Docker Creation](/media/docker-images.png)

Now you can check your browser should be serve your images with spicified ports correctly

![Docker id browser](/media/docker-id-browser.png)

## Setting Nginx Up


