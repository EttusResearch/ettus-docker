# ettus/buildbot-worker:ubuntu-18.04-oe

# please follow docker best practices
# https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/

FROM        ubuntu:18.04
LABEL       maintainer="Ettus Research"

# Last build date - this can be updated whenever there are security updates so
# that everything is rebuilt
ENV         security_updates_as_of 2020-04-20

# This will make apt-get install without question
ARG         DEBIAN_FRONTEND=noninteractive

# Install Apache Web Server
RUN apt-get update && apt-get install -y \
# yocto base dependencies
# https://www.yoctoproject.org/docs/3.0.2/ref-manual/ref-manual.html#detailed-supported-distros
    gawk \
    wget \
    git \
    diffstat \
    unzip \
    texinfo \
    gcc-multilib \
    build-essential \
    chrpath \
    socat \
    cpio \
    python \
    python3 \
    python3-pip \
    python3-pexpect \
    xz-utils \
    debianutils \
    iputils-ping \
    python3-git \
    python3-jinja2 \
    libegl1-mesa \
    libsdl1.2-dev \
    pylint3 \
    xterm \
# Other deps
    bc \
    curl \
    gnupg \
    openssl \
    hostname \
    procps \
    libc-dev-bin \
    libssl-dev \
    locales \
    net-tools \
    iproute2 \
    python-pip \
    python-dev \
    python3-dev \
    python-requests \
    python3-requests \
    rsync \
    tar \
    tmux \
    zip \
&& rm -rf /var/lib/apt/lists/*

RUN useradd -u 2017 -ms /bin/bash oe-builder

# Set UTF-8 Locale for bitbake
# See https://wiki.yoctoproject.org/wiki/TipsAndTricks/ResolvingLocaleIssues
RUN locale-gen en_US.UTF-8
RUN update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
RUN echo "export LANG=en_US.utf-8" > /opt/export_LANG.sh
ENV BASH_ENV=/opt/export_LANG.sh \
    ENV=/opt/export_LANG.sh \
    PROMPT_COMMAND="source /opt/export_LANG.sh"

RUN git config --system user.name "Ettus OE Builder" && git config --system user.email "oe-builder@ettus.com"
RUN curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > repo
RUN chmod a+x repo
RUN mv repo /usr/local/bin
USER oe-builder
WORKDIR /home/oe-builder
USER oe-builder

VOLUME /data /cache
