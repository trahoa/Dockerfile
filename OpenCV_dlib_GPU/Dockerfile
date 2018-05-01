FROM nvidia/cuda:9.0-cudnn7-devel
MAINTAINER TRAN Hoang Tung <tran-hoang.tung@usth.edu.vn>

ENV OPENCV_VERSION='3.4.1'

# Install dependencies
RUN \
    # Keep Ubuntu up to date
    apt-get update && apt-get upgrade -y && \

    # Build tools 
    apt-get install -y build-essential cmake curl && \

    # GUI
    apt-get install -y qt5-default libvtk6-dev && \

    # Media I/O
    apt-get install -y zlib1g-dev libjpeg-dev libwebp-dev libpng-dev libtiff5-dev libjasper-dev libopenexr-dev libgdal-dev && \

    # Video I/O
    apt-get install -y libdc1394-22-dev libavcodec-dev libavformat-dev libswscale-dev libtheora-dev libvorbis-dev libxvidcore-dev libx264-dev yasm libopencore-amrnb-dev libopencore-amrwb-dev libv4l-dev libxine2-dev && \

    # Parallelism and linear algebra libraries
    apt-get install -y libtbb-dev libeigen3-dev && \

    # support to HDF5
    apt-get install -y libhdf5-dev && \

    # Python
    apt-get install -y python-dev python-tk python-numpy python3-dev python3-tk python3-numpy && \

    # Java
    apt-get install -y ant default-jdk && \

    # Documentation:
    apt-get install -y doxygen && \

    # sudo
    apt-get install -y sudo && \
    
    # Cleanup
    apt-get autoclean && apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR /home

# Download OpenCV and install
RUN \
    # Download OpenCV and OpenCV-contrib
    mkdir -p opencv/build && \
    curl -L https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.tar.gz | tar xz -C opencv --strip-components 1 && \
    mkdir opencv_contrib && \
    curl -L https://github.com/opencv/opencv_contrib/archive/${OPENCV_VERSION}.tar.gz | tar xz -C opencv_contrib --strip-components 1 && \

    opencv_cmake_flags="-D CMAKE_BUILD_TYPE=RELEASE \
    -D WITH_QT=ON \
    -D WITH_OPENGL=ON \
    -D FORCE_VTK=ON \
    -D WITH_TBB=ON \
    -D WITH_GDAL=ON \
    -D WITH_XINE=ON \
    -D WITH_CUDA=ON \
    -D ENABLE_FAST_MATH=1 \
    -D CUDA_FAST_MATH=1 \
    -D WITH_CUBLAS=1 \
    -D BUILD_EXAMPLES=ON \
    -D ENABLE_PRECOMPILED_HEADERS=OFF \
    -D BUILD_PERF_TESTS=OFF \
    -D BUILD_TESTS=OFF \
    -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules/" && \
    cd opencv/build && \
    cmake $opencv_cmake_flags .. && \
    make -j $(getconf _NPROCESSORS_ONLN) && \
    make install && \
    ldconfig

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
