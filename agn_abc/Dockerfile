FROM ubuntu:18.04
MAINTAINER Ilya Pashchenko <in4pashchenko@gmail.com>

ENV DEBIAN_FRONTEND noninteractive
ENV REFRESHED_AT 17-02-2019
WORKDIR /home
VOLUME ["/data"]

# Enable multiverse repo
RUN apt-get update && \
    apt-get install -y \
        wget \
        tar \
        unzip \
        gcc \
        make \
        build-essential \

        # finufft deps
        libfftw3-dev \
        gfortran \
        numdiff \

        # polychord deps
        libopenmpi-dev \
        openmpi-bin \

        git && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install Intel distribution for python
ENV PATH /intelpython3/bin:$PATH
RUN \
    /usr/bin/wget -nv --no-check-certificate https://www.dropbox.com/s/7tme7ny7990wstk/l_pythoni3_p_2019.1.058.tar.gz && \
    /bin/tar zxvf l_pythoni3_p_2019.1.058.tar.gz && \
#    /bin/rm l_pythoni3_p_2019.1.058.tar.gz && \
    cd intelpython3  && \
    /bin/bash setup_intel_python.sh && \
    conda create -yn agn_abc && \
    source activate agn_abc

RUN \
    # scikit-image brings bad python - install it via pip
    conda install -c intel scikit-learn astropy pybind11 && \
    pip install scikit-image

# Install finufft
WORKDIR /home
RUN \
    /git clone https://github.com/flatironinstitute/finufft.git && \
    cd finufft && \
    /usr/bin/make test && \
    test/finufft1d_basicpassfail; echo $? &&\
    /usr/bin/make lib &&\
    # finufftpy has numpy in deps and brings it's own
    pip install . --no-deps


# Install polychord
WORKDIR /home
RUN \
    /git clone https://github.com/PolyChord/PolyChordLite.git && \
    cd PolyChordLite && \
    /usr/bin/make pypolychord && \
    CC=mpicc CXX=mpicxx python3 setup.py install && \

# Fetch ve
WORKDIR /home
RUN \
    /git clone https://github.com/ipashchenko/ve.git

## Fetch agn_abc
#WORKDIR /home
#RUN \
#    /git clone https://github.com/aplavin/agn_abc.git
