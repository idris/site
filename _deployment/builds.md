---
title: "Builds"
order: 290
---

Every time you deploy an application a new Build is automatically created and released. You can also create a new Build that is will not be released until manually promoted.

## Creating Builds

* Run `convox build`

A release is created, but must be manually promoted with `convox release promote <id>`.

## Listing Builds

* Run `convox builds`

## Copying Builds Between Apps

* Run `convox builds copy <id> <dest-app>`

## Exporting / Importing Builds Between Racks

* Run `convox builds export <id> > /tmp/b.tgz`
* Run `convox builds import /tmp/b.tgz`
