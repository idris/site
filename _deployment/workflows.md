---
title: "Custom Workflows"
order: 1100
---

The Build, Release and Run APIs enable you to build custom workflows. Here we demonstrate how to use Convox as a private Continuous Integration system.

## Two Racks

First we set up Staging and Production Racks with the same app on both. Staging will perform all the builds and tests and Production will only be updated with verified builds.

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
3. Export the verified Build artifact from Staging
4. Import the verified Build artifact to Production
5. Release the verified Build into Production

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

# Run tests on Staging and export a Build Artifact if they pass

convox run web make test --release $RELEASE_ID
convox builds export $BUILD_ID > /tmp/b.tgz

# Import the Build Artifact from Staging and Release on Production

convox switch production
BUILD_ID=$(convox builds import /tmp/b.tgz)
RELEASE_ID=$(convox releases | grep $BUILD_ID | cut -d" " -f1)
convox releases promote $RELEASE_ID --wait
```