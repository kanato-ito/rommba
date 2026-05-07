FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Tokyo

# 基本パッケージ
RUN apt-get update && apt-get install -y \
    locales \
    curl \
    gnupg2 \
    lsb-release \
    software-properties-common \
    git \
    && locale-gen en_US en_US.UTF-8 \
    && update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8

ENV LANG=en_US.UTF-8

# ROS 2 リポジトリ追加
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key | apt-key add - \
    && echo "deb http://packages.ros.org/ros2/ubuntu $(lsb_release -cs) main" \
    > /etc/apt/sources.list.d/ros2.list

# ROS 2 Humble インストール
RUN apt-get update && apt-get install -y \
    ros-humble-desktop \
    python3-colcon-common-extensions \
    python3-rosdep \
    && rm -rf /var/lib/apt/lists/*

# rosdep 初期化
RUN rosdep init && rosdep update

# YDlidar ドライバのインストール
RUN cd /tmp && \
    git clone https://github.com/YDLIDAR/YDLidar-SDK.git && \
    cd YDLidar-SDK && \
    mkdir build && \
    cd build && \
    cmake .. && \
    make && \
    make install

RUN apt-get update && apt-get install -y build-essential swig python3-dev python3-pip
RUN cd /tmp/YDLidar-SDK && pip3 install . && \
    python3 setup.py build && python3 setup.py install


# YDLIDAR ROS2ドライバのビルド
RUN mkdir -p /root/ydlidar_ws/src && \
    cd /root/ydlidar_ws/src && \
    git clone https://github.com/YDLIDAR/ydlidar_ros2_driver.git && \
    cd ydlidar_ros2_driver && \
    git checkout humble

RUN . /opt/ros/humble/setup.sh && \
    cd /root/ydlidar_ws && \
    rosdep install --from-paths src --ignore-src -r -y && \
    colcon build

# X4-Pro用パラメータ設定（fixed_resolutionをfalseに変更してwarningを抑制）
RUN sed -i 's/fixed_resolution: true/fixed_resolution: false/' \
    /root/ydlidar_ws/src/ydlidar_ros2_driver/params/X4-Pro.yaml

# 環境設定
RUN echo "source /opt/ros/humble/setup.bash" >> /root/.bashrc && \
    echo "source /root/ydlidar_ws/install/setup.bash" >> /root/.bashrc

ENV ROS_DOMAIN_ID=1

CMD ["/bin/bash"]