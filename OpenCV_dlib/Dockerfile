FROM thtung1/opencv:latest
MAINTAINER Tran Hoang Tung <tran-hoang.tung@usth.edu.vn>

ARG RUNTIME_DEPS='ca-certificates libpng-dev libjpeg-dev libgif-dev libopenblas-dev libx11-dev'
ARG BUILD_DEPS='apt-utils wget unzip cmake build-essential pkg-config'
ARG LIB_PREFIX='/usr/local'

ENV DLIB_VERSION='19.10' \
    LIB_PREFIX=${LIB_PREFIX} \
    DLIB_INCLUDE_DIR='$LIB_PREFIX/include' \
    DLIB_LIB_DIR='$LIB_PREFIX/lib'

# Build is based on image m03geek/ffmpeg-opencv-dlib
# https://github.com/SkeLLLa/docker-ffmpeg-opencv-dlib/tree/master/stretch

RUN echo "Dlib: ${DLIB_VERSION}" \
    && apt-get update && apt-get install -y ${BUILD_DEPS} ${RUNTIME_DEPS} --no-install-recommends \
    && wget -q https://github.com/davisking/dlib/archive/v${DLIB_VERSION}.zip -O dlib.zip \
    && dlib_cmake_flags="-D CMAKE_BUILD_TYPE=RELEASE \
    -D CMAKE_INSTALL_PREFIX=$LIB_PREFIX \
    -D DLIB_NO_GUI_SUPPORT=OFF \
    -D DLIB_USE_BLAS=ON \
    -D DLIB_GIF_SUPPORT=ON \
    -D DLIB_PNG_SUPPORT=ON \
    -D DLIB_JPEG_SUPPORT=ON \
    -D DLIB_USE_CUDA=OFF" \
    && unzip -qq dlib.zip \
    && mv dlib-${DLIB_VERSION} dlib \
    && rm dlib.zip \
    && cd dlib \
    && mkdir -p build \
    && cd build \
    && cmake $dlib_cmake_flags .. \
    && make -j $(getconf _NPROCESSORS_ONLN) \
    && make install \
    && cp ../dlib/*.txt $LIB_PREFIX/include/dlib/ \
    && cd /root \
    && rm -rf /dlib \
    && apt-get autoclean && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
    # && apt-get purge -y --auto-remove $BUILD_DEPS \
    # && apt-get autoremove -y --purge \
    # && apt-get install -y $RUNTIME_DEPS --no-install-recommends \
    # && rm -rf /var/lib/apt/lists/* /usr/share/man /usr/local/share/man /tmp/*
