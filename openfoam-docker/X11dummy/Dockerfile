FROM ubuntu:focal

#https://stackoverflow.com/questions/39085462/xdummy-in-docker-container

ENV DEBIAN_FRONTEND noninteractive
ENV DISPLAY :1

RUN apt-get update \
    && apt-get -qy --no-install-recommends --no-install-suggests install xserver-xorg-video-dummy x11-apps \
    && apt-get clean \
    && apt-get autoclean

VOLUME /tmp/.X11-unix

COPY xorg.conf /etc/X11/xorg.conf

CMD ["/usr/bin/Xorg", "-noreset", "+extension", "GLX", "+extension", "RANDR", "+extension", "RENDER", "-logfile", "./xdummy.log", "-config", "/etc/X11/xorg.conf", ":1"]
