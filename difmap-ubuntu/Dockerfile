FROM ubuntu:latest
MAINTAINER Ilya Pashchenko <in4pashchenko@gmail.com>

ENV DEBIAN_FRONTEND noninteractive
WORKDIR /home

# Enable multiverse repo
RUN apt-get update && \
    apt-get install -y \
        software-properties-common \
        python-software-properties  \
        wget \
        gcc \
        gfortran \
        make \
        python-pip && \
    apt-get update && \
    apt-add-repository multiverse && \
    apt-get update && \
    apt-get install -y pgplot5 && \
    apt-get clean

# upgrade pip
RUN pip install --upgrade pip
RUN wget ftp://"anonymous":"email"@ftp.astro.caltech.edu/pub/difmap/difmap2.4l.tar.gz && \
    tar -xvzf difmap2.4l.tar.gz && \
    cd uvf_difmap && \
    ln -s /usr/lib/x86_64-linux-gnu/libX11.so.6 /usr/lib/x86_64-linux-gnu/libX11.so && \
    ln -s /lib/x86_64-linux-gnu/libncurses.so.5 /lib/x86_64-linux-gnu/libncurses.so && \
    sed -i '/PGPLOT_LIB=\"-lpgplot -lX11\"/c\PGPLOT_LIB=\"-L/usr/lib -lpgplot -L/usr/lib/x86_64-linux-gnu -lX11 -L/lib/x86_64-linux-gnu -lncurses\"' ./configure && \
    ./configure linux-i486-gcc && \
    ./makeall && \
    echo "PATH=\$PATH:/home/uvf_difmap" >> /root/.bashrc && \
    echo "export PATH" >> /root/.bashrc

