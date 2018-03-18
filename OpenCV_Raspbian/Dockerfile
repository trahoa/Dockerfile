FROM resin/rpi-raspbian:stretch
MAINTAINER TRAN Hoang Tung <tran-hoang.tung@usth.edu.vn>

WORKDIR /root

# Install dependencies
RUN \
    # Keep Ubuntu up to date
    apt-get update && apt-get upgrade -y && \

    # Build tools 
    apt-get install -y build-essential cmake
