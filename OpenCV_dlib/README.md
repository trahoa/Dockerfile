# OpenCV and dlib

## Usage

- Enable display so that container could access it
    ```bash
    xhost +
    ```
- Create a new container and mount the home directory. Replace `<CONTAINER_NAME>` by a name that you love.

    ```bash
    docker run -it --name <CONTAINER_NAME> \
        --user $(id -u) -e DISPLAY --workdir=$(pwd) \
        --volume="/home/$USER:/home/$USER" \
        --volume="/etc/group:/etc/group:ro" \
        --volume="/etc/passwd:/etc/passwd:ro" \
        --volume="/etc/shadow:/etc/shadow:ro" \
        --volume="/etc/sudoers.d:/etc/sudoers.d:ro" \
        --volume="/tmp/.X11-unix:/tmp/.X11-unix" \
        thtung1/opencv_dlib
    ```
- Detach using `Ctrl+p`->`Ctrl+q`.
- Attach to a running docker container: `docker attach --sig-proxy=false <CONTAINER_NAME>`
- Start an existing docker container: `docker start <CONTAINER_NAME>`


## References

- [opencv and dlib for alpine](https://github.com/SkeLLLa/docker-ffmpeg-opencv-dlib)
