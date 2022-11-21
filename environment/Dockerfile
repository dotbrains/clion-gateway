FROM ubuntu:20.04

########################################################
# Essential packages for remote debugging and login in
########################################################

USER root

ENV DEBIAN_FRONTEND=noninteractive

RUN apt update \
  && apt upgrade -y \
  && apt install --no-install-recommends -y \
    apt-utils \
    openssh-server \
    ssh \
    sudo \
    gcc \
    g++ \
    gdb \
    gdbserver \
    valgrind \
    make \
    clang \
    cmake \
    rsync \
    curl \
    wget \
    tar \
    python3 \
    git \
    nano \
    vim \
    locales \
    locales-all \
    default-jre \
    gnupg2 \
    libarchive-zip-perl \
    tmux \
  && apt clean autoclean \
  && apt autoremove --yes \
  && rm -rf /var/lib/{apt,dpkg,cache,log}

RUN unset DEBIAN_FRONTEND

ADD . /code
WORKDIR /code

# Add necessary permissions to /code
RUN chmod -R 777 /code

# Taken from - https://docs.docker.com/engine/examples/running_ssh_service/#environment-variables

# Change root password.
RUN echo 'root:root' | chpasswd

# Configure ssh.
RUN mkdir -p /var/run/sshd

# Allow root login via ssh.
RUN sed -i 's/#\?PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config

# Allow ssh password authentication.
RUN sed -i 's/#\?PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config

# SSH login fix. Otherwise user is kicked off after login.
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

RUN ( \
    echo 'PermitRootLogin yes'; \
    echo 'PasswordAuthentication yes'; \
    echo 'Subsystem sftp /usr/lib/openssh/sftp-server'; \
  ) > /etc/ssh/sshd_config_jetbrains

# Create non-root user.
RUN useradd -m -s /bin/bash --create-home --base-dir /home student \
  && mkdir -p /home/student/.ssh \
  && yes student | passwd student

# Expose port 22 for SSH server.
EXPOSE 22

# Expose 7777 for gdb server.
EXPOSE 7777

# Use docker-bash-rc file to set up bash and make it pretty.
ADD docker-etc-profile.sh /etc/docker-etc-profile.sh
RUN chmod 777 /etc/docker-etc-profile.sh
RUN echo "source /etc/docker-etc-profile.sh" >> /root/.bashrc
RUN echo "source /etc/docker-etc-profile.sh" >> /home/student/.bashrc

# Setup locale.
ENV LC_ALL=en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US.UTF-8

# Set up tmux.
COPY tmux.conf /root/.tmux.conf
COPY tmux /root/.tmux
RUN git clone https://github.com/tmux-plugins/tpm /root/.tmux/plugins/tpm

COPY tmux.conf /home/student/.tmux.conf
COPY tmux /home/student/.tmux
RUN git clone https://github.com/tmux-plugins/tpm /home/student/.tmux/plugins/tpm

########################################################
# Add custom packages and development environment here
########################################################

# add custom packages here

########################################################

CMD ["/usr/sbin/sshd", "-D", "-e", "-f", "/etc/ssh/sshd_config_jetbrains"]
