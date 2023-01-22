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

It's crucial to ensure that your web applications are always available and can handle high traffic. That's where a load balancer comes in - it distributes incoming traffic evenly across multiple servers, maximizing performance and minimizing downtime.

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

Now if you checked your browser for every port should be serve your images with spicified ports correctly. for example port 3001

![Docker id browser](/media/docker-id-browser.png)

and so on.

## Setting Nginx Up

After Installing Nginx to your machine, we will work with `/etc/nginx/nginx.conf`. Copy code below to your `nginx.conf` file

```bash
http {
    upstream containers {
        server 127.0.0.1:3001;
        server 127.0.0.1:3002;
        server 127.0.0.1:3003;
        server 127.0.0.1:3004;
        server 127.0.0.1:3005;
    }
    server {
        listen 80;
        location / {
            proxy_pass http://containers/;
        }
    }
}

events {}
```

Now we Created upstream called containers to serve all our backend and we also created server listen to port `80` and proxy_pass to our containers upstream.

If we visit `localhost` we should see nginx balancing and switching between created containters.

![balancer on browser](/media/balancer.gif)

You can notice that it serve in sequential order that is because nginx uses [Round Robin Algorithm](https://en.wikipedia.org/wiki/Round-robin_scheduling)

## Conclusion

In conclusion, creating a load balancer with Docker and NGINX is a powerful way to ensure the availability and performance of your web application. By distributing incoming traffic evenly across multiple servers, a load balancer can maximize performance and minimize downtime.
