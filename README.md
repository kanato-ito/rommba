# rommba
## dockerコマンド
Linuxの場合

  xhost +local:docker

  docker run -it \
    --device=/dev/ttyUSB0:/dev/ttyUSB0 \
    --group-add dialout \
    -e DISPLAY=$DISPLAY \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    your_image