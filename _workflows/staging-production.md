---
title: "Staging and Production"
---

A Convox best practice is to have two Racks, one for staging and one for production. 

On the isolated staging rack you will:

* Deploy small-scale versions apps for review or QA
* Perform new builds
* Run automated tests
* Test migrations
* Apply `convox rack update` frequently

On the isolated production rack you will:

* Scale apps for production traffic
* Import verified builds
* Run verified migrations
* Apply `convox rack update` during safe maintenance windows

Workflows make it easy to deliver apps from staging to production.

## Staging and Production Racks

First we set up Staging and Production Racks with the same app name on both.

```
$ convox install --stack-name staging
$ convox apps create myapp

$ convox install --stack-name production
$ convox apps create myapp

$ convox racks
RACK                              STATUS
personal/staging                  running
personal/production               running
```

## CI Workflow

Now we design a simple CI workflow:

1. Build and Release on Staging
2. Pass automated tests on Staging
3. Pass automated migrations on Staging
4. Export the verified Build artifact from Staging
5. Import the verified Build artifact to Production
6. Release the verified Build into Production

## Script

We can script this workflow using the `convox` CLI tool and the Build, Release and Run APIs:

```bash
#!/bin/bash
set -ex

# Deploy to Staging and capture the Build and Release Id

convox switch staging
BUILD_ID=$(convox build --id)
RELEASE_ID=$(convox releases | grep $BUILD_ID | cut -d" " -f1)
convox releases promote $RELEASE_ID --wait

# Run tests and migrations on Staging and export a Build Artifact if they pass

convox run web rake test --release $RELEASE_ID
convox run web rake db:migrate --release $RELEASE_ID
convox builds export $BUILD_ID > /tmp/b.tgz

# Import the Build Artifact from Staging and Release on Production

convox switch production
RELEASE_ID=$(convox builds import /tmp/b.tgz --id)
convox releases promote $RELEASE_ID --wait
```