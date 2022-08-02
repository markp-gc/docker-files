FROM graphcore/poplar:2.6.0-ubuntu-20.04-20220729
  LABEL version="0.1.0"
  LABEL description="This container is intended for development work on the Graphcore Monte-Carlo ray tracer example."

  # Tell apt never to prompt while we're doing a Dockerfile build
  ARG DEBIAN_FRONTEND=noninteractive

  ARG partition_id
  ARG vipu_host
  ARG CUSTOM_SSH_PORT=44300

  # Setup environment variables:
  ENV LC_ALL=C.UTF-8
  ENV GCDA_MONITOR=1
  ENV OPENCV_IO_ENABLE_OPENEXR=1
  ENV IPUOF_CONFIG_PATH=/etc/ipuof.conf.d/partition.conf
  ENV TERM=xterm-256color

  # Install dependencies needed for development:
  RUN apt-get update && apt-get install -y \
    build-essential gdb ninja-build nasm llvm \
    git-lfs libboost-all-dev libeigen3-dev libhdf5-dev libopencv-dev libopenexr-dev libspdlog-dev \
    imagemagick pfstools libavformat-dev graphviz \
    python3 python3-pip virtualenv \
    rsync openssh-server nmap \
    sudo vim nano bash-completion

RUN sed -i 's/\(^Port\)/#\1/' /etc/ssh/sshd_config && echo Port $CUSTOM_SSH_PORT >> /etc/ssh/sshd_config

# Setup a new sudo user 'ubuntu' instead of running the container as root:
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN useradd -rm -d /home/ubuntu -s /bin/bash -G sudo ubuntu
RUN echo ubuntu:ubuntu | chpasswd
RUN mkdir -p /home/ubuntu/workspace && chown ubuntu:ubuntu /home/ubuntu/workspace
RUN mkdir -p /home/ubuntu/workspace/reports && chown ubuntu:ubuntu /home/ubuntu/workspace/reports

USER ubuntu
WORKDIR /home/ubuntu/workspace
# Install cmake via pip to get latest version:
RUN echo "export PATH=/home/ubuntu/.local/bin:${PATH}" >> /home/ubuntu/.bashrc
RUN pip install -U pip
RUN pip install cmake

VOLUME /home/ubuntu

RUN sudo service ssh start
RUN sudo service ssh force-reload

CMD sudo service ssh start && /bin/bash
