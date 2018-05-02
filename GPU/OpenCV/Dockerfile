FROM thtung1/gpu_base
MAINTAINER TRAN Hoang Tung <tran-hoang.tung@usth.edu.vn>

ENV OPENCV_VERSION='3.4.1'

# Download OpenCV and install
RUN \
    # Download OpenCV and OpenCV-contrib
    mkdir -p opencv/build && \
    curl -L https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.tar.gz | tar xz -C opencv --strip-components 1 && \
    mkdir opencv_contrib && \
    curl -L https://github.com/opencv/opencv_contrib/archive/${OPENCV_VERSION}.tar.gz | tar xz -C opencv_contrib --strip-components 1 && \

    opencv_cmake_flags="-D CMAKE_BUILD_TYPE=RELEASE \
    -D WITH_CUDA=ON \
    -D ENABLE_FAST_MATH=1 \
    -D CUDA_FAST_MATH=1 \
    -D WITH_CUBLAS=1 \
    -D WITH_QT=ON \
    -D WITH_OPENGL=ON \
    -D FORCE_VTK=ON \
    -D WITH_TBB=ON \
    -D WITH_GDAL=ON \
    -D WITH_XINE=ON \
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
