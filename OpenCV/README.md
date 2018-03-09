# OpenCV 3.4.1 for Ubuntu 16.04
OpenCV 3.4.1 + opencv_contrib + Python {2,3} + Java + GUI + media/video support + HDF5

## Usage

- Enable display and make a new directory to mount to the container
    ```bash
    xhost +
    mkdir ~/Documents/OpenCV
    cd ~/Documents/OpenCV
    ```
- Create a new container and mount the current folder to `/root/OpenCV`:

    `docker run --name <CONTAINER_NAME> -e DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v $(pwd):/root/OpenCV -it thtung1/OpenCV`
- Detach using `Ctrl+p`->`Ctrl+q`.
- Attach to a running docker container: `docker attach --sig-proxy=false <CONTAINER_NAME>`
- Start an existing docker container: `docker start <CONTAINER_NAME>`

## Compile an OpenCV example

The original instruction is [here](https://docs.opencv.org/3.4.1/db/df5/tutorial_linux_gcc_cmake.html)

- write a simple code `DisplayImage.cpp`
    ```C++
    #include <stdio.h>
    #include <opencv2/opencv.hpp>
    using namespace cv;
    int main(int argc, char** argv )
    {
        if ( argc != 2 )
        {
            printf("usage: DisplayImage.out <Image_Path>\n");
            return -1;
        }
        Mat image;
        image = imread( argv[1], 1 );
        if ( !image.data )
        {
            printf("No image data \n");
            return -1;
        }
        namedWindow("Display Image", WINDOW_AUTOSIZE );
        imshow("Display Image", image);
        waitKey(0);
        return 0;
    }
    ```
- Create a CMakeList.txt or a [more detail verion](https://github.com/opencv/opencv/blob/master/samples/cpp/example_cmake/CMakeLists.txt)
    ```cmake
    cmake_minimum_required(VERSION 2.8)
    project( DisplayImage )
    find_package( OpenCV REQUIRED )
    include_directories( ${OpenCV_INCLUDE_DIRS} )
    add_executable( DisplayImage DisplayImage.cpp )
    target_link_libraries( DisplayImage ${OpenCV_LIBS} )
    ```
- Build the program
    ```bash
    mkdir build
    cd build
    cmake ..
    make
    ```
- Test the program `./DisplayImage whatever_image.jpg`

## References

- [Docker Full Description Example](https://hub.docker.com/r/victorhcm/opencv/)
- [Dockerfile format](https://github.com/victorhcm/dockerfiles/blob/master/opencv/Dockerfile)
- [Install OpenCV script](https://github.com/milq/milq/blob/master/scripts/bash/install-opencv.sh)
- [CMakeLists.txt from OpenCV](https://github.com/opencv/opencv/blob/master/samples/cpp/example_cmake/CMakeLists.txt)
- [OpenCV CMake configuration file](https://github.com/opencv/opencv/blob/master/cmake/templates/OpenCVConfig.cmake.in)
