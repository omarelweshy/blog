---
date: 2022-12-17 11:14:00 GMT+2
categories: [Backend, DevOps]
tags: [Nginx, Docker]
comments: true
image:
  src: /media/github.png
  width: 800
  height: 533
  alt: Github repo sync dotfiles
---

## Introduction

In this post we will create Load balancer with Nignx and Docker Containers

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
})

app.listen(3000, () => console.log("app at http://localhost:3000"))

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


inprogress .........
