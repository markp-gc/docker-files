FROM graphcore/tensorflow:2-amd-2.6.0-ubuntu-20.04-20220729
  LABEL version="0.2.0"
  LABEL description="This container is intended for development work on the Graphcore Monte-Carlo path-tracer example."

  # Tell apt never to prompt while we're doing a Dockerfile build
  ARG DEBIAN_FRONTEND=noninteractive

  ARG UNAME=ubuntu
  ARG UID=1000
  ARG GID=1000
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
    build-essential gdb ninja-build nasm \
    git-lfs libboost-all-dev libeigen3-dev libhdf5-dev libopencv-dev libopenexr-dev libspdlog-dev \
    imagemagick pfstools libavformat-dev \
    python3 python3-pip virtualenv \
    rsync openssh-server nmap curl \
    sudo vim nano bash-completion

RUN sed -i 's/\(^Port\)/#\1/' /etc/ssh/sshd_config && echo Port $CUSTOM_SSH_PORT >> /etc/ssh/sshd_config

# Setup a new sudo user '$UNAME' instead of running the container as root:
RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN groupadd -g $GID -o $UNAME
RUN useradd -rm -u $UID -g $GID -d /home/$UNAME -s /bin/bash -G sudo $UNAME
RUN echo $UNAME:$UNAME | chpasswd
RUN mkdir -p /home/$UNAME/workspace && chown $UNAME:$UNAME /home/$UNAME/workspace

USER $UNAME
WORKDIR /home/$UNAME/workspace
# Install cmake via pip to get latest version:
RUN echo "export PATH=/home/$UNAME/.local/bin:${PATH}" >> /home/$UNAME/.bashrc
RUN pip3 install cmake Pygments

# Copy a VSCode auto-complete file into the container:
COPY c_cpp_properties.json /opt/vscode/c_cpp_properties.json

VOLUME /home/$UNAME

RUN sudo service ssh start
RUN sudo service ssh force-reload

CMD sudo service ssh start && /bin/bash
