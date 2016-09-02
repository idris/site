---
title: "Custom"
---

The Build, Release and Run APIs enable you to build custom workflows. 

Here we demonstrate how to use two separate apps, a staging app for testing and review and a production app for releasing only verified builds.

## Two Apps

First we set up two apps, one for staging and one for production.

```
$ convox install
$ convox apps create myapp-staging
$ convox apps create myapp-production
```

## CI Workflow

Now we design a simple CI workflow:

1. Build and Release on the staging app
2. Pass automated tests on the staging app
3. Copy the verified build artifact to the production app
4. Release the verified build on the production app

## Script

We can script this workflow using the `convox` CLI tool and the Build, Release and Run APIs:

```bash
#!/bin/bash
set -ex

# Deploy to the staging app and capture the Build and Release Id

BUILD_ID=$(convox build --id --app myapp-staging)
RELEASE_ID=$(convox releases --app myapp-staging | grep $BUILD_ID | cut -d" " -f1)
convox releases promote $RELEASE_ID --wait --app myapp-staging

# Run tests on the staging app and export a Build Artifact if they pass

convox run web make test --release $RELEASE_ID --app myapp-staging
convox builds export $BUILD_ID --app myapp-staging > /tmp/b.tgz

# Copy the Build from Staging and Release on Production

convox builds copy $BUILD_ID myapp-production --promote --app myapp-staging
```