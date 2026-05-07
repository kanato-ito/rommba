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

docker run -it --network=host --device=/dev/ttyUSB0:/dev/ttyUSB0 --group-add dialout rommba:latest

## LIDAR launch
ros2 launch ydlidar_ros2_driver ydlidar_launch.py \
    params_file:=/root/ydlidar_ws/src/ydlidar_ros2_driver/params/X4-Pro.yaml
