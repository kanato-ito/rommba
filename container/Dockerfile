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


# 環境設定
RUN echo "source /opt/ros/humble/setup.bash" >> /root/.bashrc

CMD ["/bin/bash"]