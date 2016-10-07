---
title: "Introduction"
permalink: guide/introduction
---

The goal is simple: take an app and run it forever in the cloud.

We have great news: all the tools and services we need to do this already exist.

The only catch: we have to understand and follow some guidelines.

We start with understanding how to build good Images on our laptop as part of the development process.

We continue by carefully adding configuration that describes the rest of things our application needs, like processes, balancers and databases.

We maintain this forever by mapping this app configuration onto simple, "off-the-shelf" cloud services, and automating changes to these components.

This book guides you through the steps to build and run any app in this fashion.

It takes 30 minutes to read.

We recommend that you read through it once to learn the concepts and see what you are already familar with and what conventiosn your app already follow. Then we recommend you go back to the beginning with one of your apps and the `convox doctor` tool to build, run, develop and deploy your app to production.

## Philosophy

### Open over Closed

This guide and the accompanying tools are open source. We believe you should be able to inspect and modify the software you depend on to run your business. We believe in open standards and transparency.

### Integration over Invention

This guide follows existing best practices. We believe in using technology that already exists whenever possible. We would much rather consume a service than run a piece of software.

### Robots over Humans

This guide demonstrates a repeatable process. We believe in automation. We believe that we can design a system for low maintenance and perpetual availability, and take the worry away from us humans.

## Who Is This Guide For?

This guide is for application developers that are ready to learn how to take total control of the development and production systems that power your app.

## Pre-Requesites

* Convox
* Docker
* AWS

Run `convox doctor` to test your setup.

```bash
$ convox doctor

### Pre-Requisites
[✓] Docker running
[✓] AWS credentials
```

## The Process

1. [Build -- Build Images](guide/build)
    * [Images](guide/images)
    * [Processes](guide/processes)
    * [Environment](guide/environment)
2. Run -- Run an App as a Formation of Images
    * Balancers
    * Services
    * Links
3. Develop -- Interactively Make and Verify Changes to an App
    * Volumes
    * Migrations
    * Tests
    * Consoles
    * Reloading
4. Deploy -- Safely Deploy Changes to the Cloud
    * Health Checks
    * Logs
    * Databases
5. Automate -- Customize the Deployment Workflow
    * Webhooks
    * Build Import / Export
    * Releases