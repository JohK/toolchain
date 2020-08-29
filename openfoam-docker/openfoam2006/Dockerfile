# Copyright (c) Johannes Kneer

ARG BASE_CONTAINER=openfoamplus/of_v2006_centos73:latest
FROM $BASE_CONTAINER

MAINTAINER Johannes Kneer johannes.kneer@nuflo.de

ENV PATH /opt/rh/rh-python38/root/usr/bin:/opt/rh/rh-python38/root/usr/local/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin


RUN yum update -y && yum clean all && \
    yum install -y centos-release-scl && \
    yum install -y rh-python38 rh-python38-python-pip gnuplot


RUN scl enable rh-python38 bash
RUN pip install numpy pyFoam matplotlib pandas

#RUN echo "PATH=/opt/rh/rh-python38/root/usr/local/bin:${PATH}" > /etc/environment
