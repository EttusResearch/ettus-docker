# ettus/buildbot-worker:fedora-25-oe

# please follow docker best practices
# https://docs.docker.com/engine/userguide/eng-image/dockerfile_best-practices/

FROM        fedora:25
MAINTAINER  Ettus Research

# Last build date - this can be updated whenever there are security updates so
# that everything is rebuilt
ENV         security_updates_as_of 2017-04-27


# Install Apache Web Server
RUN dnf install -y \
    gawk \
    make \
    wget \
    tar \
    bzip2 \
    gzip \
    python \
    unzip \
    findutils \
    which \
    perl \
    patch \
    diffutils \
    diffstat \
    bc \
    cpio \
    file \
    git \
    cpp \
    gcc \
    gcc-c++ \
    gnupg \
    glibc-devel \
    texinfo \
    chrpath \
    openssl \
    ccache \
    perl-Data-Dumper \
    perl-bignum \
    perl-Text-ParseWords \
    perl-Thread-Queue \
    SDL-devel \
    xterm \
    iputils \
    hostname \
    procps \
    net-tools \
    iproute \
    python-pip \
    python-devel \
    python-requests \
    redhat-rpm-config \
    openssl-devel \
    rpm-devel \
    rsync \
    tar \
    tmux \
    xz \
    && \
    dnf clean all


RUN useradd -u 2017 -ms /bin/bash oe-builder

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
