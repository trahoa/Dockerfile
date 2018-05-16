FROM thtung1/gpu_base:cuda8
MAINTAINER TRAN Hoang Tung <tran-hoang.tung@usth.edu.vn>

RUN cd opencv/build && \
    cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D WITH_QT=ON \
    -D CUDA_GENERATION=Pascal \
    -D WITH_OPENGL=ON \
    -D FORCE_VTK=ON \
    -D WITH_TBB=ON \
    -D WITH_GDAL=ON \
    -D WITH_XINE=ON \
    -D BUILD_EXAMPLES=ON \
    -D ENABLE_PRECOMPILED_HEADERS=OFF \
    -D BUILD_PERF_TESTS=OFF \
    -D BUILD_TESTS=OFF \
    -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules/ .. && \
    make -j $(getconf _NPROCESSORS_ONLN) && \
    make install && \
    ldconfig
