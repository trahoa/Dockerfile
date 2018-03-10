FROM ubuntu:16.04
MAINTAINER TRAN Hoang Tung <tran-hoang.tung@usth.edu.vn>

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

    # Cleanup
    apt-get autoclean && apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*


# Download OpenCV 3.4.1 and install
RUN \
    # Download OpenCV and OpenCV-contrib
    cd /root && \
    mkdir opencv && \
    curl -L https://github.com/opencv/opencv/archive/3.4.1.tar.gz | tar xz -C opencv --strip-components 1 && \
    mkdir opencv_contrib && \
    curl -L https://github.com/opencv/opencv_contrib/archive/3.4.1.tar.gz | tar xz -C opencv_contrib --strip-components 1 && \

    cd opencv && mkdir build && cd build && \
    cmake -DCMAKE_BUILD_TYPE=RELEASE -DWITH_QT=ON -DWITH_OPENGL=ON -DFORCE_VTK=ON -DWITH_TBB=ON -DWITH_GDAL=ON -DWITH_XINE=ON -DBUILD_EXAMPLES=ON -DENABLE_PRECOMPILED_HEADERS=OFF -DBUILD_PERF_TESTS=OFF -DBUILD_TESTS=OFF -DOPENCV_EXTRA_MODULES_PATH=~/opencv_contrib/modules .. && \
    make && \ # Change to "make -j $(($(nproc) - 1)) && \" if you run on your PC
    make install && \
    ldconfig

WORKDIR /root
