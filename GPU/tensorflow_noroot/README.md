# Python 2
Tensorflow 1.8.0 with CUDA 9 and OpenCV 3.4.1 (compiled without CUDA support)

In this image, OpenCV 3.4.1 could not be compiled with CUDA 9 because of some conflicts between CUDA-of-tensorflow and OpenCV. However, OpenCV 3.4.1 could be installed with `nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04`. This needs further investigation.

## Usage

```bash
docker run --runtime=nvidia -it --name nlp \
      --user $(id -u) -e DISPLAY --workdir=$(pwd) \
      --volume="/home/$USER:/home/$USER" \
      --volume="/etc/group:/etc/group:ro" \
      --volume="/etc/passwd:/etc/passwd:ro" \
      --volume="/etc/shadow:/etc/shadow:ro" \
      --volume="/etc/sudoers.d:/etc/sudoers.d:ro" \
      --volume="/tmp/.X11-unix:/tmp/.X11-unix" \
      -p 8888:8888 -p 6006:6006 \
       thtung1/tensorflow_noroot:1.8.0-devel-gpu
```

Inside docker, you can run jupyter with the following command
> `jupyter lab --ip=0.0.0.0`
