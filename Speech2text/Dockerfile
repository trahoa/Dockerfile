FROM nvidia/cuda:10.1-cudnn7-devel-ubuntu18.04

WORKDIR /root

ENV LANG="C.UTF-8"
ENV MKLROOT=/opt/intel/mkl

RUN apt-get update && \
    apt-get install -y software-properties-common && \
    add-apt-repository ppa:mhier/libboost-latest && \
    apt-get update && \
    apt-get install -y libboost-all-dev && \
    apt-get install -y build-essential git cmake libfreeimage-dev cmake-curses-gui && \
    apt-get remove --purge -y cmake && \
    apt-get install -y build-essential libssl-dev sudo wget && \
    wget https://cmake.org/files/v3.16/cmake-3.16.5.tar.gz && \
    tar -zxvf cmake-3.16.5.tar.gz && \
    rm cmake-3.16.5.tar.gz && \
    cd cmake-3.16.5 && ./bootstrap && make && \
    sudo make install && \
    wget https://dl.bintray.com/boostorg/release/1.70.0/source/boost_1_70_0.tar.gz && \
    tar xf boost_1_70_0.tar.gz && \
    cp -r boost_1_70_0/boost /usr/local/include && \
    apt-get install -y openmpi-bin openmpi-common libopenmpi-dev && \
    ln -s /usr/lib/x86_64-linux-gnu/libcublas.so.10.2.1.243 /usr/local/cuda/lib64/libcublas.so && \
    ln -s /usr/lib/x86_64-linux-gnu/libcublas.so.10.2.1.243 /usr/local/cuda/lib64/libcublas.so.10 && \
    ln -s /usr/lib/x86_64-linux-gnu/libcublasLt.so.10.2.1.243 /usr/local/cuda/lib64/libcublasLt.so && \
    ln -s /usr/lib/x86_64-linux-gnu/libcublasLt.so.10.2.1.243 /usr/local/cuda/lib64/libcublasLt.so.10 && \
    ln -s /usr/local/cuda/lib64/stubs/libcuda.so /usr/lib/x86_64-linux-gnu/libcuda.so.1 && \
    apt-get autoremove -y

# arrayfire
RUN git clone --recursive https://github.com/arrayfire/arrayfire.git --branch v3.6.4 && \
    apt-get install -y libglfw3-dev libgl1-mesa-dev libglu1-mesa-dev
RUN apt-get install -y libopenblas-dev libfftw3-dev liblapacke-dev libatlas-base-dev && \
    cd arrayfire && mkdir build && cd build && \
    cmake .. -DCMAKE_BUILD_TYPE=Release -DAF_BUILD_CUDA=ON && \
    make -j$(nproc) && make install

# Gloo
RUN git clone https://github.com/facebookincubator/gloo && \
    cd gloo && git checkout b2ff1aa716dd37be5000dd3f13b214610c87d22f && \
    mkdir -p build && cd build && \
    apt-get install -y openmpi-bin openmpi-common libopenmpi-dev && \
    cmake .. -DUSE_MPI=ON && make -j$(nproc) && make install

# MKL
RUN wget https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB && \
    apt-key add GPG-PUB-KEY-INTEL-SW-PRODUCTS-2019.PUB && \
    sh -c 'echo deb https://apt.repos.intel.com/mkl all main > /etc/apt/sources.list.d/intel-mkl.list' && \
    apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends cpio intel-mkl-64bit-2018.4-057

# MKL_DNN
RUN git clone https://github.com/intel/mkl-dnn.git -b mnt-v0 && \
    cd mkl-dnn && mkdir -p build && cd build && \
    cmake .. && make -j$(nproc) && make install

# flashlight
RUN git clone https://github.com/facebookresearch/flashlight && \
    cd flashlight && git checkout 16d0bf0bf1dfb930b7bb0415260f9e7914f7818c && \
    mkdir -p build && cd build && \
    cmake .. -DCMAKE_BUILD_TYPE=Release -DFLASHLIGHT_BACKEND=CUDA && \
    make -j$(nproc) && make install

# KENLM
RUN apt-get install -y libsndfile-dev libopenblas-dev libfftw3-dev libgflags-dev libgoogle-glog-dev && \
    apt-get install -y liblzma-dev libbz2-dev libzstd-dev && \
    apt-get install -y libeigen3-dev && \
    git clone https://github.com/kpu/kenlm.git && \
    cd kenlm && mkdir -p build && cd build && \
    cmake .. -DKENLM_MAX_ORDER=20 && \
    make -j$(nproc) && make install

ENV KENLM_ROOT_DIR=/root/kenlm

# wav2letter from mailong
RUN apt-get install -y libhdf5-dev && \
    git clone https://github.com/mailong25/wav2letter.git && \
    cd wav2letter && \
    sed -i 's+<fl::InlineReducer>(1.0 / fl::getWorldSize())+<fl::CoalescingReducer>(1.0 / fl::getWorldSize(),true,true)+g' Train.cpp && \
    mkdir -p build && cd build && \
    cmake .. -DCMAKE_BUILD_TYPE=Release -DKENLM_MAX_ORDER=20 -DW2L_LIBRARIES_USE_CUDA=ON && \
    make -j$(nproc)
