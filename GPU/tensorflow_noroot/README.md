# Tensorflow 2.0.0 with CUDA 10 and OpenCV 4.1.1

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
       thtung1/tensorflow_noroot
```

Inside docker, you can run jupyter with the following commands:
> `export SHELL=/bin/bash`

> `jupyter lab --ip=0.0.0.0 --no-browser`
