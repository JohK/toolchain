FROM ubuntu:focal 
MAINTAINER Johannes Kneer johannes.kneer@nuflo.de

USER root
ENV DEBIAN_FRONTEND=noninteractive

RUN echo ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true | debconf-set-selections && \
    apt-get update && apt-get install -qy --no-install-recommends --no-install-suggests \
           build-essential autoconf autotools-dev cmake gawk gnuplot \
	   flex libfl-dev libreadline-dev zlib1g-dev openmpi-bin libopenmpi-dev mpi-default-bin mpi-default-dev \
           libgmp-dev libmpfr-dev libmpc-dev libvtk7-dev mesa-utils \
           vim curl wget sudo gmsh gnuplot make g++ ttf-mscorefonts-installer\
           python3 python3-pip python3-vtk7 python3-pyqt5 python3-numpy python3-matplotlib python3-tk python3-scipy \
           python3-pandas python3-gmsh python3-gnuplot python3-xlwt python3-xlrd git mercurial bear bison \
           gcc g++ gfortran cmake cmake-curses-gui libboost-dev libboost-all-dev libblas-dev liblapack-dev \
           libopenmpi-dev openmpi-bin openmpi-doc xorg-dev libmotif-dev gsl-bin libgsl-dev libc++abi-dev libhdf5-dev && \
    apt-get clean && apt-get autoclean && \
    update-alternatives --install /usr/bin/python python /usr/bin/python3 20

RUN pip3 --no-cache-dir install PyFoam parsim pyvista

RUN useradd --user-group --create-home --shell /bin/bash --groups sudo --uid 1001 ofuser ;\
	echo "ofuser ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# install openfoam
RUN curl -s https://dl.openfoam.com/add-debian-repo.sh | bash ;\
	apt-get update && apt-get install -y openfoam2006-default;\
	rm -rf /var/lib/apt/lists/* ;\
	echo "source /usr/lib/openfoam/openfoam2006/etc/bashrc" >> ~ofuser/.bashrc ;\
	echo "export OMPI_MCA_btl_vader_single_copy_mechanism=none" >> ~ofuser/.bashrc

# install the visualization module
# requires VTK lib
RUN mkdir /home/ofuser/src && chown ofuser:ofuser /home/ofuser/src && git clone https://develop.openfoam.com/modules/visualization.git /home/ofuser/src/visualization && \
    /usr/bin/openfoam2006 -c 'WM_NCOMPPROCS=12 VTK_DIR="/usr/include/vtk-7.1" /home/ofuser/src/visualization/Allwmake -prefix=/usr/lib/openfoam/openfoam2006/platforms/linux64GccDPInt32Opt' ;\
    rm -rf /home/ofuser/src/visualization

# install cfmesh?
RUN git clone https://develop.openfoam.com/Community/integration-cfmesh.git /home/ofuser/src/cfmesh && \
    /usr/bin/openfoam2006 -c 'WM_NCOMPPROCS=12 /home/ofuser/src/cfmesh/Allwmake -prefix=/usr/lib/openfoam/openfoam2006/platforms/linux64GccDPInt32Opt' ;\
    rm -rf /home/ofuser/src/cfmesh


# install swak4Foam
# req
# bear, bison, wget
USER ofuser
RUN hg clone http://hg.code.sf.net/p/openfoam-extend/swak4Foam /home/ofuser/src/swak4Foam && cd /home/ofuser/src/swak4Foam && hg update develop && \
    /usr/bin/openfoam2006 -c 'WM_NCOMPPROCS=12 /home/ofuser/src/swak4Foam/AllwmakeAll -prefix=/usr/lib/openfoam/openfoam2006/platforms/linux64GccDPInt32Opt' && \
    rm -rf /home/ofuser/src/swak4Foam
USER root

# install dakota
# req
#           gcc g++ gfortran cmake cmake-curses-gui libboost-dev libboost-all-dev libblas-dev liblapack-dev \
#           libopenmpi-dev openmpi-bin openmpi-doc xorg-dev libmotif-dev gsl-bin libgsl-dev libc++abi-dev libhdf5-dev && \
RUN mkdir -p /home/ofuser/src/dakota/build && \
    curl -s https://dakota.sandia.gov/sites/default/files/distributions/public/dakota-6.12-release-public.src.tar.gz | tar -xz -C /home/ofuser/src/dakota && \
    cd /home/ofuser/src/dakota/build && \
    cmake -DCMAKE_INSTALL_PREFIX=/opt/dakota \
          -DCMAKE_C_FLAGS="-O2" \
          -DCMAKE_CXX_FLAGS="-O2" \
          -DCMAKE_Fortran_FLAGS="-O2" \
          -DHAVE_QUESO:BOOL=ON \
          -DDAKOTA_HAVE_GSL:BOOL=ON \
          -DDAKOTA_HAVE_MPI:BOOL=TRUE \
          -DDAKOTA_HAVE_NPSOL:BOOL=ON \
          -DDAKOTA_HDF5:BOOL=TRUE \
          -DBoost_NO_BOOST_CMAKE:BOOL=TRUE \
          -DCMAKE_CXX_COMPILER:FILEPATH=/usr/bin/mpicxx \
          /home/ofuser/src/dakota/dakota-6.12.0.src ;\
    cat /home/ofuser/src/dakota/build/CMakeFiles/CMakeOutput.log ;\
    cat /home/ofuser/src/dakota/build/CMakeFiles/CMakeError.log && \
    make -j16 && \
    make install && \
    rm -rf /home/ofuser/src/dakota ; \
    echo "export PATH="\$PATH:/opt/dakota/bin:/opt/dakota/share/dakota/test:/opt/dakota/gui >> ~ofuser/.bashrc ;\
    echo "export PYTHONPATH=\$PYTHONPATH:/opt/dakota/share/dakota/Python" >> ~ofuser/.bashrc



USER ofuser
WORKDIR /home/ofuser
