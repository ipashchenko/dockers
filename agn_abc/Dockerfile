FROM ubuntu:18.04
MAINTAINER Ilya Pashchenko <in4pashchenko@gmail.com>

ENV DEBIAN_FRONTEND noninteractive
ENV REFRESHED_AT 17-02-2019
WORKDIR /home
VOLUME ["/data"]

# This can be used instead of RUN
#ENTRYPOINT ["/bin/bash", "-c"]
#CMD ["source activate your-environment && exec python application.py"]

# Enable multiverse repo
RUN apt-get update && \
    apt-get install -y \
        wget \
        tar \
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
#ENV PATH /intelpython3/bin:$PATH
RUN \
    /usr/bin/wget -nv --no-check-certificate https://www.dropbox.com/s/7tme7ny7990wstk/l_pythoni3_p_2019.1.058.tar.gz && \
    /bin/tar zxvf l_pythoni3_p_2019.1.058.tar.gz
RUN \
#    /bin/rm l_pythoni3_p_2019.1.058.tar.gz && \
    cd intelpython3  && \
    /bin/bash setup_intel_python.sh
RUN \
    /home/intelpython3/bin/conda create -yn agn_abc

ENV PATH /home/intelpython3/bin:$PATH

RUN \
    echo $PATH
RUN \
    conda env list

RUN \
    conda install -c intel --name agn_abc scikit-learn astropy pybind11 matplotlib

# Some way to pip-install inside conda
RUN /bin/bash -c ". activate agn_abc && pip install scikit-image --no-deps"

# Install finufft
WORKDIR /home
RUN \
    git clone https://github.com/flatironinstitute/finufft.git && \
    cd finufft && \
    /usr/bin/make test && \
    test/finufft1d_basicpassfail; echo $? &&\
    /usr/bin/make lib &&\
    # finufftpy has numpy in deps and brings it's own
    /bin/bash -c ". activate agn_abc && pip install . --no-deps"


## This install w/o activating intel env but uses pip outside of the conda
#WORKDIR /home
#RUN git clone https://github.com/flatironinstitute/finufft && \
#    cd ./finufft && \
#    CPATH=/home/intelpython3/include/:$CPATH LIBRARY_PATH=/home/intelpython3/lib/:$LIBRARY_PATH make test -j && \
#    CPATH=/home/intelpython3/include/:$CPATH LIBRARY_PATH=/home/intelpython3/lib/:$LIBRARY_PATH make lib -j && \
#    CPATH=/home/intelpython3/include/:$CPATH LIBRARY_PATH=/home/intelpython3/lib/:$LIBRARY_PATH pip install . --no-deps && \
#    cd ../ && rm -rf finufft


# Install polychord
WORKDIR /home
RUN \
    git clone https://github.com/PolyChord/PolyChordLite.git && \
    cd PolyChordLite && \
    /usr/bin/make pypolychord && \
    /bin/bash -c ". activate agn_abc && CC=mpicc CXX=mpicxx python3 setup.py install"

# Fetch ve
# Update sklearn (GMM, etc)
WORKDIR /home
RUN \
    git clone https://github.com/ipashchenko/ve.git

## Fetch agn_abc
#WORKDIR /home
#RUN \
#    /git clone https://github.com/aplavin/agn_abc.git
