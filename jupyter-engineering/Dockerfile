# Copyright (c) Johannes Kneer

ARG BASE_CONTAINER=jupyter/scipy-notebook
FROM $BASE_CONTAINER

LABEL maintainer="Johannes Kneer <johannes.kneer@gmail.com>" 
USER root


RUN apt-get update -q && apt-get install -qy --no-install-recommends --no-install-suggests \
        texlive-latex-base texlive-latex-recommended texlive-science texlive-latex-extra texlive-fonts-recommended dvipng ghostscript && \
    rm -rf /var/lib/apt/lists/* && \
    conda install --quiet --yes \
	'astropy' \
	'geopandas' \
	'cartopy' \
	'cantera::cantera' \
	'conda-forge::uncertainties=3.0*' && \
	#'menpo::opencv' && \
	conda clean -tipsy && \
	pip install --no-cache-dir https://github.com/PyMesh/PyMesh/releases/download/v0.2.1/pymesh2-0.2.1-cp36-cp36m-linux_x86_64.whl && \
	fix-permissions $CONDA_DIR && \
	fix-permissions /home/$NB_USER

#fenics?
#pymesh?
#ADD https://github.com/PyMesh/PyMesh/releases/download/v1.2.1/pymesh2-0.2.1-cp36-cp36m-linux_x86_64.whl /root/pymesh-0.2.1.whl

USER $NB_UID
