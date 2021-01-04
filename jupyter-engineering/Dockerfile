# Copyright (c) Johannes Kneer

ARG BASE_CONTAINER=jupyter/scipy-notebook:latest
FROM $BASE_CONTAINER

LABEL maintainer="Johannes Kneer <johannes.kneer@gmail.com>" 
USER root

RUN echo ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true | sudo debconf-set-selections && \
    apt-get update -q && apt-get install -qy --no-install-recommends --no-install-suggests \
        git xfoil texlive-latex-base texlive-latex-recommended texlive-science texlive-latex-extra texlive-fonts-recommended dvipng ghostscript ttf-mscorefonts-installer && \
    rm -rf /var/lib/apt/lists/*

RUN conda install --quiet --yes \
	'conda-forge::astropy' \
	'geopandas' \
	'cartopy' \
	#'cantera::cantera' \
	'conda-forge::numpy' \
	'conda-forge::scipy' \
	'conda-forge::numba' \
	'conda-forge::uncertainties' \ 
	'conda-forge::jupyter_nbextensions_configurator' \
	'conda-forge::jupyterlab-git' \
	'conda-forge::jupyterlab_latex' \
#	'conda-forge::jupyterlab-python-file' \
	'conda-forge::jupyterlab_code_formatter' \
	'conda-forge::voila' \
	'conda-forge::appmode' \
	#'menpo::opencv' && \
	'conda-forge::opencv' \
	'conda-forge::fenics-dolfin' && \
	conda clean -tipsy

#COPY viiflow-1.6-cp36-cp36m-linux_x86_64.whl /tmp/viiflow-1.6-cp36-abi3-linux_x86_64.whl
#COPY viiflow-1.6-cp38-cp38-macosx_10_13_x86_64.whl /tmp/
#COPY viiflowtools-1.6-py3-none-any.whl /tmp/

RUN pip install --no-cache-dir \
#https://github.com/PyMesh/PyMesh/releases/download/v0.2.1/pymesh2-0.2.1-cp36-cp36m-linux_x86_64.whl \
	coolprop \
	tables \
	ipympl \
	pyromat \
	pvlib \
	fenics \
	solcore \	
	cookiecutter \
	pythreejs \
	pint \
	pyFoam \
	airfoils \
        plotly \
        jupyter-dash && \ 
    pip install git+https://github.com/leal26/AeroPy.git  && \
#    pip install /tmp/viiflow-1.6-cp36-abi3-linux_x86_64.whl && \
    #pip install /tmp/viiflow-1.6-cp38-cp38-macosx_10_13_x86_64.whl && \
#    pip install /tmp/viiflowtools-1.6-py3-none-any.whl && \
    pip install --upgrade jupyterlab-git
    #pip install --no-deps --force-reinstall --upgrade --install-option="--with_pdd" solcore && \
RUN jupyter labextension install \ 
        @lckr/jupyterlab_variableinspector \
	@jupyter-widgets/jupyterlab-manager \
@jupyterlab/toc \
	@jupyter-voila/jupyterlab-preview \
	jupyter-threejs \
#	@jupyterlab/git \
	@jupyterlab/celltags \
#	@jupyterlab/latex \
	jupyter-matplotlib && \
    jupyter serverextension enable --sys-prefix jupyterlab_latex && \
    jupyter lab build && \
    jupyter labextension list && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER 

# solcore needs to be installed twice so dependencies don't fail

#fenics?
#pymesh?
#ADD https://github.com/PyMesh/PyMesh/releases/download/v1.2.1/pymesh2-0.2.1-cp36-cp36m-linux_x86_64.whl /root/pymesh-0.2.1.whl

USER $NB_UID
