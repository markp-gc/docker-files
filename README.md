# docker-files

Various docker files organised by base image:

- graphcore: images built ontop of [Graphcore](https://hub.docker.com/u/graphcore) base images.

Typical build and run command templates for Graphcore images:

```
docker build -t new_image <path-to-dockerfile>
gc-docker run -it --rm -v <path-to-vipu-config>:/etc/ipuof.conf.d/ new_image
```

Inside the containers you will be a generic user named ubuntu with sudo privileges.
You can edit the Docker script before building if you prefer a different username.

To use gc-docker you need a Poplar SDK installed and activated:
[Graphcore developer site](https://www.graphcore.ai/developer).
Note: gc-docker does not need to be from the same SDK version used in the
container so if your machine came with Poplar pre-installed you will only
need to activate the installation (see Graphcore's documentation).
