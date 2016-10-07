---
title: "Processes"
---

## What

Once you have recipes for building the image(s) that make up your application, you can start to define how those images are composed into a coherent application. In general, your application will be executed as one or more processes that are run from the images you created. For example, you may have a `web` process that handles incoming HTTP requests and a `worker` process that executes background jobs from a queue. In most cases, these processes are simply different commands run against the same image, but they can be run against different images that you build or even images built by others and pulled from a public source like Docker Hub.

## Why

Composing your app of many discrete processes has several advantages.

It allows you to scale your app in several dimensions. You might need 5 `web` processes to handle incoming HTTP traffic, but only 2 `worker` processes, to handle the job queue, for example.

Multiple processes can be distributed across servers in different datacenters and geographic locations, giving your application redundancy.

Stateless processes are disposable.

## How

You use the `docker-compose.yml` file format to describe to Convox how your application should be run. The following example builds on our previous Node.js example. Here we're running 2 different types of processes, a `web` and a `worker`. Both of these processes use the same image. The `build: .` line says that the process is run based on the image built from the Dockerfile in the current directory. Since `web` doesn't define a command the default command from the Dockerfile (`npm start`) is used. The `worker` process needs a different command to run a worker, so it's specified here using the `command` setting.

```yaml
version: '2'
services:
  web:
    build: .
  worker:
    build: .
    command: ["npm", "work"]
```

## Test

Try writing the `docker-compose.yml` file needed to build your images from your Dockerfile and run them with the necessary commands. Once your file is written, you can use `convox doctor` to check it for problems.

## Troubleshooting


