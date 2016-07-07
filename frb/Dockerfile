FROM ubuntu:16.04
MAINTAINER Ilya Pashchenko <in4pashchenko@gmail.com>

ENV DEBIAN_FRONTEND noninteractive
ENV REFRESHED_AT 07-07-2016
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
        make \
        libfreetype6-dev \
        libxft-dev \
        python-pip \
        python-matplotlib \
        python-scipy \
        python-astropy \
        python-h5py \
        python-sqlalchemy \
        git && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN \
    pip2 install scikit-image scikit-learn

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
    /usr/bin/wget -nv --no-check-certificate https://github.com/ipashchenko/frb/archive/dev.zip && \
    /usr/bin/unzip dev.zip && \
    rm dev.zip && \
    cd frb-dev/my5spec && \
    /usr/bin/make
