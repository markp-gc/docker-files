FROM graphcore/tensorflow:2-amd-3.3.0-ubuntu-20.04-20230703
  LABEL version="0.7.0"
  LABEL description="This container is intended for running and developing Poplar applications in an Ubuntu-20 environment."

  # Tell apt never to prompt while we're doing a Dockerfile build
  ARG DEBIAN_FRONTEND=noninteractive

  ARG UNAME=ubuntu
  ARG UID=1000
  ARG GID=1000
  ARG partition_id
  ARG vipu_host
  ARG CUSTOM_SSH_PORT=2023

  # Setup environment variables:
  ENV LC_ALL=C.UTF-8
  ENV GCDA_MONITOR=1
  ENV OPENCV_IO_ENABLE_OPENEXR=1
  ENV TERM=xterm-256color

  # Install dependencies needed for development:
  RUN apt-get update && apt-get install -y \
    build-essential gdb ninja-build nasm llvm cmake \
    git-lfs libboost-all-dev libeigen3-dev libhdf5-dev libopencv-dev libopenexr-dev libspdlog-dev \
    imagemagick pfstools libavformat-dev graphviz \
    libembree-dev libglfw3-dev libassimp-dev libopenimageio-dev \
    python3 python3-pip python3-opencv python3-matplotlib virtualenv \
    rsync openssh-server nmap \
    sudo vim nano bash-completion \
    curl clang-tidy-10 doxygen \
    libjpeg8-dev libssl-dev \
    python3-sphinx swig

RUN sed -i 's/\(^Port\)/#\1/' /etc/ssh/sshd_config && echo Port $CUSTOM_SSH_PORT >> /etc/ssh/sshd_config

# Workaround for boost 1.71 CMake find_package issue:
RUN ln -s /usr/include /include

# Setup a new sudo user '$UNAME' instead of running the container as root:
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN groupadd -g $GID -o $UNAME
RUN useradd -rm -u $UID -g $GID -d /home/$UNAME -s /bin/bash -G sudo $UNAME
RUN echo $UNAME:$UNAME | chpasswd

# Install cmake via pip to get latest version:
RUN pip install -U pip
RUN pip install cmake matplotlib

VOLUME /home/$UNAME
USER $UNAME
WORKDIR /home/$UNAME

RUN sudo service ssh start
RUN sudo service ssh force-reload
CMD sudo service ssh start && /bin/bash
