# Docker Image
base: aws-lambda-python-313
python: 3.13
node: 23.x
poetry: 1.8.4
yarn: 1.22.22

## How to create
docker buildx create --name aws-lambda-python-313 --driver docker-container --bootstrap

## Run test
docker buildx build --platform linux/amd64,linux/arm64 -t whaikung/aws-lambda-python-313 .

## Build and push to hub docker repo
docker buildx build --platform linux/amd64,linux/arm64 -t whaikung/aws-lambda-python-313 --push .
