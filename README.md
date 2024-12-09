https://docs.docker.com/build/building/multi-platform/

```
# To build a multi-arch image, instantiate a custom buildx
docker buildx create --name mybuilder --driver docker-container --bootstrap
docker buildx use mybuilder

# Login is required to push to repo
docker login
# Make sure to switch to mybuilder; builder default may
# change upon docker restart
docker buildx use mybuilder
# Build (for testing)
docker buildx build --platform linux/amd64,linux/arm64 -t <username>/<repository> .
# Build and push to hub docker repo
docker buildx build --platform linux/amd64,linux/arm64 -t <username>/<repository> --push .

# Example 1: build and push to docker hub
docker buildx build --platform linux/amd64,linux/arm64 -t whaikung/aws-lambda-python-312 --push .
# Example 2: Build only and output to build.log:
# https://forums.docker.com/t/capture-ouput-of-docker-build-into-a-log-file/123178/2
docker buildx build --platform linux/amd64,linux/arm64 -t whaikung/aws-lambda-python-312 . 2>&1 | tee build.log
```

https://circleci.com/docs/2.0/custom-images/#creating-a-custom-image-manually
