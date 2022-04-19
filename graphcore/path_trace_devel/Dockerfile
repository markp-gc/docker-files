FROM graphcore/tensorflow
  MAINTAINER Graphcore <@graphcore.ai>

  # Tell apt never to prompt while we're doing a Dockerfile build
  ARG DEBIAN_FRONTEND=noninteractive

  # Setup environment variables:
  ENV LC_ALL=C.UTF-8
  ENV GCDA_MONITOR=1
  ENV OPENCV_IO_ENABLE_OPENEXR=1
  ENV IPUOF_CONFIG_PATH=/etc/ipuof.conf.d/partition.conf
  ENV TERM=xterm-256color

  # Install dependencies needed for development:
  RUN apt-get update && apt-get install -y \
    cmake \
    git-lfs \
    imagemagick \
    libboost-all-dev \
    libeigen3-dev \
    libhdf5-dev \
    libopencv-dev \
    libopenexr-dev \
    libspdlog-dev \
    ninja-build \
    pfstools \
    python3 \
    python-pip \
    sudo

# Setup a new sudo user 'ubuntu' instead of running the container as root:
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN useradd -rm -d /home/ubuntu -s /bin/bash -G sudo ubuntu
USER ubuntu
WORKDIR /home/ubuntu/
VOLUME /home/ubuntu/workspace

CMD /bin/bash
