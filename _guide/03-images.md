---
title: "Images"
permalink: guide/images
---

[Previous: Introduction]() [Next: Processes]()

An Image is an immutable package containing your app and its dependencies, including the operating system and language tools.

Using an Image enables you to run your app predictably on any computer, from your laptop to the cloud.

## Building an Image

Images are defined in a Dockerfile.

A Dockerfile starts from a base Image and includes the steps necessary to build your app.

The following Dockerfile defines an Image for an example Node.js app:

```Dockerfile
# start from a base Image
FROM ubuntu:16.04

# install system dependencies
RUN apt-get update && apt-get install -y node npm

# specify the app location
WORKDIR /app

# install app dependencies
COPY package.json /app
RUN npm install

# add app source code
COPY . /app
```

## Try It

Run `convox doctor` to build and validate your Image.

```bash
$ convox doctor

### Build
[✓] Dockerfile found 
[✓] Image built sucessfully
```

Now that you have a working Image, let's [define a Process](/guide/processes).

## Further Reading

* [Choosing a base image]()
* [Vendoring private dependencies]()
* [Speeding up builds]()

[Previous: Introduction]() [Next: Processes]()
