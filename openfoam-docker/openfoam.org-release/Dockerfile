# Copyright (c) Johannes Kneer

ARG BASE_CONTAINER=openfoam/openfoam7-graphical-apps:latest
FROM $BASE_CONTAINER

LABEL maintainer="Johannes Kneer <johannes.kneer@gmail.com>" 
USER root

ARG TZ=Europe/Berlin 
ARG DEBIAN_FRONTEND=noninteractive
RUN echo ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true | debconf-set-selections && \
    apt-get update -q && apt-get install -qy --no-install-recommends --no-install-suggests \
        texlive-latex-base texlive-latex-recommended texlive-science texlive-latex-extra texlive-fonts-recommended \
        dvipng \
        ghostscript \
	gnuplot \
        python3.6 python3-pip python3-numpy python3-matplotlib python3-pandas python3-vtk7 \
#python3-gnuplotlib python3-gnuplot python3-pygnuplot \
        ttf-mscorefonts-installer && \
    rm -rf /var/lib/apt/lists/*


RUN update-alternatives  --install /usr/bin/python python /usr/bin/python3.6 10 && update-alternatives  --install /usr/bin/python3 python3 /usr/bin/python3.6 10 && \
    pip3 install pyFoam



USER 98765
