FROM ubuntu:latest
MAINTAINER Ilya Pashchenko <in4pashchenko@gmail.com>

ENV DEBIAN_FRONTEND noninteractive
ENV REFRESHED_AT 18-01-2016
WORKDIR /home
VOLUME ["/data"]

# Enable multiverse repo
RUN apt-get update && \
    apt-get install -y \
        libfftw3-dev \
        wget \
        tar \
        unzip \
        gcc \
        make && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN \
    /usr/bin/wget -nv --no-check-certificate https://www.dropbox.com/s/j0cfvrxro6t95uy/mark5access-1.4.5.tar.gz && \
    /bin/tar zxvf mark5access-1.4.5.tar.gz && \
    /bin/rm mark5access-1.4.5.tar.gz && \
    cd mark5access-1.4.5 && \
    ./configure && \
    /usr/bin/make && \
    /usr/bin/make install
RUN echo "/usr/local/lib" >> /etc/ld.so.conf &&\
    /sbin/ldconfig
WORKDIR /home
RUN \
    /usr/bin/wget -nv --no-check-certificate https://github.com/voitsik/my5spec/archive/master.zip && \
    /usr/bin/unzip master.zip && \
    rm master.zip && \
    cd my5spec-master && \
    /usr/bin/make
