FROM tikazyq/crawlab:0.5.1

# Install OpenCV
ENV OPENCV_VERSION='4.2.0'
# Install dependencies
RUN apt update && \
    apt install -y build-essential cmake curl && \
    apt install -y pkg-config \
    libjpeg-dev libpng-dev libtiff-dev \
    gfortran libatlas-base-dev \
    libtbb-dev libeigen3-dev && \
    # Cleanup
    apt-get autoclean && apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
     # Download OpenCV and OpenCV-contrib
RUN pip install numpy==1.20.1 && \
    mkdir -p opencv/build && \
    curl -L https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.tar.gz | tar xz -C opencv --strip-components 1 && \
    mkdir opencv_contrib && \
    curl -L https://github.com/opencv/opencv_contrib/archive/${OPENCV_VERSION}.tar.gz | tar xz -C opencv_contrib --strip-components 1 && \
    cd opencv/build && \
    cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D ENABLE_CXX11=ON \
    -D WITH_INF_ENGINE=ON \
    -D WITH_QT=OFF \
    -D WITH_CUDA=OFF \
    -D WITH_OPENGL=OFF \
    -D WITH_OPENCL=OFF \
    # -D FORCE_VTK=ON \
    -D WITH_TBB=ON \
    -D WITH_GDAL=OFF \
    -D WITH_XINE=ON \
    -D BUILD_EXAMPLES=OFF \
    -D ENABLE_PRECOMPILED_HEADERS=OFF \
    -D BUILD_PERF_TESTS=OFF \
    -D BUILD_TESTS=OFF \
    -D OPENCV_ENABLE_NONFREE=ON \
    -D BUILD_LIST=core,python3,xfeatures2d,highgui \
    -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules/ .. && \
    make -j $(getconf _NPROCESSORS_ONLN) && \
    make install && \
    ldconfig && \
    cd ../.. && \
    rm -rf opencv && \
    rm -rf opencv_contrib
