# docker-files

Various docker files organised by base image:

- graphcore: images built ontop of [Graphcore](https://hub.docker.com/u/graphcore) base images.

Typical build and run command templtes for Graphcore images:

```
docker build -t new_image <path-to-dockerfile>
docker run -it --rm --net=host --cap-add IPC_LOCK --device=/dev/infiniband/ --ipc=host -v <path-to-vipu-config>:/etc/ipuof.conf.d/ new_image
```
