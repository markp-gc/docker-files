FROM graphcore/tensorflow
  LABEL version="0.1.0"
  LABEL description="This container is intended for development work on the Graphcore Monte-Carlo ray tracer example."

  # Tell apt never to prompt while we're doing a Dockerfile build
  ARG DEBIAN_FRONTEND=noninteractive

  # Setup environment variables:
  ENV LC_ALL=C.UTF-8
  ENV GCDA_MONITOR=1
  ENV OPENCV_IO_ENABLE_OPENEXR=1
  #ENV IPUOF_CONFIG_PATH=/etc/ipuof.conf.d/partition.conf
  ENV TERM=xterm-256color

  # Install dependencies needed for development:
  RUN apt-get update && apt-get install -y \
    build-essential gdb cmake ninja-build \
    git-lfs libboost-all-dev libeigen3-dev libhdf5-dev libopencv-dev libopenexr-dev libspdlog-dev \
    imagemagick pfstools \
    python3 python-pip virtualenv \
    rsync openssh-server nmap \
    sudo vim nano bash-completion

RUN sed -i 's/\(^Port\)/#\1/' /etc/ssh/sshd_config && echo Port 44300 >> /etc/ssh/sshd_config

# Setup a new sudo user 'ubuntu' instead of running the container as root:
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN useradd -rm -d /home/ubuntu -s /bin/bash -G sudo ubuntu
RUN echo ubuntu:ubuntu | chpasswd
RUN mkdir -p /home/ubuntu/workspace && chown ubuntu:ubuntu /home/ubuntu/workspace
RUN echo ubuntu:ubuntu | chpasswd
RUN mkdir -p /home/ubuntu/workspace/reports && chown ubuntu:ubuntu /home/ubuntu/workspace/reports
USER ubuntu
VOLUME /home/ubuntu
WORKDIR /home/ubuntu/workspace

ARG partition_id
ARG vipu_host
ENV IPUOF_VIPU_API_PARTITION_ID=$partition_id
ENV IPUOF_VIPU_API_HOST=$vipu_host

RUN sudo service ssh start
RUN sudo service ssh force-reload

CMD sudo service ssh start && /bin/bash
