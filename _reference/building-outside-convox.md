---
title: "Building Outside Convox"
---

Some workflows might benefit from running apps that use pre-built images, rather than images built by Convox at deploy time. To do so, you'll need just a handful of Docker commands and a line or two of configuration to use your images with Convox.

In the steps below, we'll assume that you have not yet built your image, that you have a Dockerfile for your app, and that you're using Docker Hub as your Docker image registry.

## Get set up on Docker Hub

If you don't already have a Docker Hub account, visit [hub.docker.com](https://hub.docker.com/) to set one up. Your Docker Hub ID will be used as the namespace (namespace/repo) for your repo in later steps. Once you've registered and are signed in, create a new repository. This is where your new image will live. Finally, log in to your docker account from the command line:

    docker login

## Build and Push

You're now ready to create an image and push it to the repository you just created. Keep in mind that repos can be public or private.

    docker build -f Dockerfile -t namespace/repo .
    docker push namespace/repo

## Use your image with Convox

To use that image (or any other) to run containers with Convox, reference it in your docker-compose.yml.

    image: namespace/repo

Now you can use `convox start` to run that image as a container locally, or `convox deploy` to run it in a Rack.

## Using tags

If you don't specify a tag when pulling from your image repo, you'll get the latest version of the image by default. That won't always be a sensible approach. For greater control, you can use tags.

### Basic tagging

Let's say you're ready to start running v3.0.2 of your app. You'd specify the tag `v3.0.2` when building your image and then push the tagged image to your repo.

    docker build -f Dockerfile -t namespace/repo:v3.0.2 .
    docker push namespace/repo:v3.0.2

Then in your docker-compose.yml, you'd declare that your application should be run using the tagged image you just built and pushed:

    image: namespace/repo:v3.0.2

### Dynamic tagging

What if you want to build your images automatically, but don't want to risk always using the latest image? One scriptable solution would be to use a tagging scheme based on timestamps.

    export TAG=$(date "+%Y%m%d%H%M%S")

    docker build -f Dockerfile -t namespace/repo:$TAG .
    docker push namespace/repo:$TAG

    # Set IMAGE_VERSION for use with `convox start`
    export IMAGE_VERSION=$TAG

    # Set IMAGE_VERSION for use with your deployed Convox app
    convox env set IMAGE_VERSION=$TAG

Then in your docker-compose.yml:

    image: namespace/repo:$IMAGE_VERSION
