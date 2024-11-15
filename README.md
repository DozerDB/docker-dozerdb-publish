# docker-dozerdb-publish

DozerDB images are available in the official Docker [image library](https://hub.docker.com/r/graphstack/dozerdb).

This repository provides Dockerfiles for published releases of the full DozerDB distribution, which is built upon Neo4j with additional DozerDB assets. 

While this repository is forked from [`neo4j/docker-neo4j`](https://github.com/neo4j/docker-neo4j), we do not make updates to the Neo4j repository itself. Instead, we modify select files here to integrate DozerDB-specific components before publishing.




# Building the images.

To build multi-platform Docker images compatible with both AMD64 and ARM architectures (including ARM v8), we use Docker’s buildx feature. The command below allows you to build and push the image to Docker Hub, tagging it with the specific DozerDB and Neo4j versions and designating it as the latest release.





```

docker buildx build \
  --build-arg CACHEBUST=$(date +%s) \
  --no-cache \
  --platform linux/amd64,linux/arm64,linux/arm64/v8 \
  --tag "graphstack/dozerdb:5.25.1.0-alpha.1" \
  --tag "graphstack/dozerdb:latest" \
  --tag "graphstack/dozerdb:5.25.1" \
  "5.25.1/bullseye/dozerdb" \
  --push

```


# Notes on building image from local distribution

If you want to build an image with a local distribution, then you may need to modify the Dockerfile line that pulls down the distribution from a remote url.
For example copying the file instead of using curl.

```
ARG NEO4J_URI=https://dist.dozerdb.org/dozerdb-5.25.1.1-alpha.1-unix.tar.gz

...

RUN apt update \
    && apt-get install -y curl gcc git jq make procps tini wget \
    && curl --fail --silent --show-error --location --remote-name ${NEO4J_URI} \

```


