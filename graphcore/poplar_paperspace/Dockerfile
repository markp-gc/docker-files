FROM graphcore/tensorflow-jupyter:2-amd-3.1.0-ubuntu-20.04-20230104
  LABEL version="0.5.0"
  LABEL description="This container is intended for Poplar application development on Paperspace."

  # Tell apt never to prompt while we're doing a Dockerfile build
  ARG DEBIAN_FRONTEND=noninteractive

  ARG UNAME=ubuntu
  ARG UID=1000
  ARG GID=1000
  ARG partition_id
  ARG vipu_host

  # Setup environment variables:
  ENV LC_ALL=C.UTF-8
  ENV GCDA_MONITOR=1
  ENV OPENCV_IO_ENABLE_OPENEXR=1
  ENV TERM=xterm-256color

  # Install dependencies needed for development:
  RUN apt-get update && apt-get install -y \
    build-essential gdb ninja-build nasm llvm \
    git-lfs libboost-all-dev libeigen3-dev libhdf5-dev libopencv-dev libopenexr-dev libspdlog-dev \
    imagemagick pfstools libavformat-dev graphviz \
    libembree-dev libglfw3-dev libassimp-dev libopenimageio-dev \
    python3 python3-pip virtualenv \
    rsync openssh-server nmap \
    sudo vim nano bash-completion \
    curl clang-tidy-10 doxygen \
    libjpeg8-dev libssl-dev \
    python3-sphinx swig

# Setup a new sudo user '$UNAME' instead of running the container as root:
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN groupadd -g $GID -o $UNAME
RUN useradd -rm -u $UID -g $GID -d /home/$UNAME -s /bin/bash -G sudo $UNAME
RUN echo $UNAME:$UNAME | chpasswd
RUN mkdir -p /notebooks/workspace && chown -R $UNAME:$UNAME /notebooks/workspace

USER $UNAME
WORKDIR /notebooks
# Make sure we can access IPUs and Poplar as this user:
RUN echo "export PATH=/home/$UNAME/.local/bin:${PATH}" >> /home/$UNAME/.bashrc
RUN echo "source /opt/poplar/enable.sh" >> /home/$UNAME/.bashrc

# Install cmake via pip to get latest version:
RUN pip3 install cmake
