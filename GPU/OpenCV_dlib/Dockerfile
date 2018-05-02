FROM thtung1/gpu_opencv
MAINTAINER TRAN Hoang Tung <tran-hoang.tung@usth.edu.vn>

ARG RUNTIME_DEPS='ca-certificates libpng-dev libjpeg-dev libgif-dev libopenblas-dev libx11-dev'
ARG BUILD_DEPS='apt-utils wget unzip cmake build-essential pkg-config'
ARG LIB_PREFIX='/usr/local'

ENV DLIB_VERSION='19.10' \
    LIB_PREFIX=${LIB_PREFIX} \
    DLIB_INCLUDE_DIR='$LIB_PREFIX/include' \
    DLIB_LIB_DIR='$LIB_PREFIX/lib'

# Build is based on image m03geek/ffmpeg-opencv-dlib
# https://github.com/SkeLLLa/docker-ffmpeg-opencv-dlib/tree/master/stretch

RUN apt-get update && apt-get install -y ${BUILD_DEPS} ${RUNTIME_DEPS} --no-install-recommends && \
    mkdir -p dlib/build && \
    curl -L https://github.com/davisking/dlib/archive/v${DLIB_VERSION}.tar.gz | tar xz -C dlib --strip-components 1 && \
    dlib_cmake_flags="-D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=$LIB_PREFIX \
    -D DLIB_NO_GUI_SUPPORT=OFF \
    -D DLIB_USE_BLAS=ON \
    -D DLIB_GIF_SUPPORT=ON \
    -D DLIB_PNG_SUPPORT=ON \
    -D DLIB_JPEG_SUPPORT=ON \
    -D DLIB_USE_CUDA=ON" && \
    cd dlib/build && \
    cmake $dlib_cmake_flags .. && \
    make -j $(getconf _NPROCESSORS_ONLN) && \
    make install && \
    cd ../examples && mkdir build && cd build && \
    cmake .. && \
    cmake --build . --config Release && \
    find . -maxdepth 1 -type f -perm -a=x -print0 | xargs -0 -I {} cp {} /usr/local/bin && \
    cd ../../tools/imglab && mkdir build && cd build && \
    cmake .. && \
    cmake --build . --config Release && \
    make install && \
    apt-get autoclean && apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
