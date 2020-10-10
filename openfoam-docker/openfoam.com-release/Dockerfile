FROM ubuntu:focal 
MAINTAINER Johannes Kneer johannes.kneer@nuflo.de

USER root
ENV DEBIAN_FRONTEND=noninteractive

RUN echo ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true | debconf-set-selections && \
    apt-get update && apt-get install -qy --no-install-recommends --no-install-suggests vim curl sudo gmsh gnuplot make g++ ttf-mscorefonts-installer\
           python3 python3-pip python3-vtk7 python3-pyqt5 python3-numpy python3-matplotlib python3-tk\
           python3-pandas python3-gmsh python3-gnuplot python3-xlwt python3-xlrd git mercurial

RUN pip3 install PyFoam

RUN useradd --user-group --create-home --shell /bin/bash --groups sudo --uid 1001 ofuser ;\
	echo "ofuser ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

RUN curl -s https://dl.openfoam.com/add-debian-repo.sh | bash ;\
	apt-get install -y openfoam2006-default;\
# openfoam2006-dev openfoam2006-tutorials openfoam2006-tools ;\
	rm -rf /var/lib/apt/lists/* ;\
	echo "source /usr/lib/openfoam/openfoam2006/etc/bashrc" >> ~ofuser/.bashrc ;\
	echo "export OMPI_MCA_btl_vader_single_copy_mechanism=none" >> ~ofuser/.bashrc

USER ofuser
WORKDIR /home/ofuser
