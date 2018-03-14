FROM resin/raspberrypi3-python:3.6.4-onbuild
MAINTAINER TRAN Hoang Tung <tran-hoang.tung@usth.edu.vn>

WORKDIR /root

# Install dependencies
RUN \
    # Keep Ubuntu up to date
    apt-get update && apt-get upgrade -y && \

    # Build tools 
    apt-get install -y build-essential cmake curl && \

    # GUI (if you want to use GTK instead of Qt, replace 'qt5-default' with 'libgtkglext1-dev' and remove '-DWITH_QT=ON' option in CMake):
    sudo apt-get install -y qt5-default libvtk6-dev
    
    # Media I/O
    apt-get install -y zlib1g-dev libjpeg-dev libwebp-dev libpng-dev libtiff5-dev libjasper-dev libopenexr-dev libgdal-dev && \

    # Video I/O
    apt-get install -y libdc1394-22-dev libavcodec-dev libavformat-dev libswscale-dev libtheora-dev libvorbis-dev libxvidcore-dev libx264-dev yasm libopencore-amrnb-dev libopencore-amrwb-dev libv4l-dev libxine2-dev && \

    # Cleanup
    apt-get autoclean && apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Download OpenCV 3.4.1 and install
RUN \
    # Download OpenCV and OpenCV-contrib
    mkdir -p opencv/build && \
    curl -L https://github.com/opencv/opencv/archive/3.4.1.tar.gz | tar xz -C opencv --strip-components 1 && \
    mkdir opencv_contrib && \
    curl -L https://github.com/opencv/opencv_contrib/archive/3.4.1.tar.gz | tar xz -C opencv_contrib --strip-components 1 && \
    
    cd opencv/build && \
    cmake -DENABLE_VFPV3=ON -DENABLE_NEON=ON -DWITH_QT=ON -DWITH_OPENGL=ON -DFORCE_VTK=ON -DWITH_GDAL=ON -DWITH_XINE=ON -DBUILD_EXAMPLES=ON -DENABLE_PRECOMPILED_HEADERS=OFF -DBUILD_PERF_TESTS=OFF -DBUILD_TESTS=OFF -DBUILD_opencv_python2=OFF -DBUILD_opencv_python3=ON -DOPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules/ .. && \
    make && \
    make install && \
    ldconfig
