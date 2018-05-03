FROM tensorflow/tensorflow:1.8.0-devel-gpu-py3
MAINTAINER TRAN Hoang Tung <tran-hoang.tung@usth.edu.vn>

WORKDIR /home

RUN \
    # Keep Ubuntu up to date
    apt-get update && apt-get upgrade -y && \
    apt-get install -y sudo && \
    apt-get autoclean && apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
