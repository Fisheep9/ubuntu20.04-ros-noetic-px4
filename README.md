# ubuntu20.04-ros-noetic-px4

环境配置

ubuntu20.04-ros-noetic-px4

[dockerfile](https://github.com/osrf/docker_images/blob/df19ab7d5993d3b78a908362cdcd1479a8e78b35/ros/noetic/ubuntu/focal/ros-base/Dockerfile)

docker 走代理

```sh
export http_proxy=http://127.0.0.1:7890
export https_proxy=http://127.0.0.1:7890
```

```dockerfile
# This is an auto generated Dockerfile for ros:ros-base
# generated from docker_images/create_ros_image.Dockerfile.em
FROM ros:noetic-ros-core-focal

# install bootstrap tools
RUN apt-get update && apt-get install --no-install-recommends -y \
    build-essential \
    python3-rosdep \
    python3-rosinstall \
    python3-vcstools \
    && rm -rf /var/lib/apt/lists/*

# bootstrap rosdep
RUN rosdep init && \
  rosdep update --rosdistro $ROS_DISTRO

# install ros packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    ros-noetic-ros-base=1.5.0-1* \
    && rm -rf /var/lib/apt/lists/*
```

```sh
docker build -t ros:noetic .
```

在ARM上直接NAS拉镜像
```sh
docker load -i ~/Downloads/hilab_v2.tar
```

启动容器

```sh
sudo docker run -it --network host --privileged \
--env="DISPLAY" \
--env="QT_X11_NO_MITSHM=1" \
--volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
ros:noetic

sudo apt-get update
sudo apt-get install vim
```

[换源](https://www.yisu.com/ask/4042.html)

[PX4安装](https://docs.px4.io/main/en/dev_setup/dev_env_linux_ubuntu.html#ros-gazebo-classic)

```sh
sudo apt update
sudo apt install git
git clone https://github.com/PX4/PX4-Autopilot.git --recursive

bash ./PX4-Autopilot/Tools/setup/ubuntu.sh
```

https://github.com/PX4/PX4-Autopilot/blob/main/Tools/setup/ubuntu.sh

重启docker

```sh
sudo apt-get install protobuf-compiler libeigen3-dev libopencv-dev -y
```

[mavros安装](https://docs.px4.io/main/en/ros/mavros_installation.html)

```sh
sudo apt-get install ros-noetic-mavros ros-noetic-mavros-extras ros-noetic-mavros-msgs
wget https://raw.githubusercontent.com/mavlink/mavros/master/mavros/scripts/install_geographiclib_datasets.sh
sudo bash ./install_geographiclib_datasets.sh   
```

rviz安装

```sh
sudo apt-get install ros-noetic-rviz
```

pcl安装

```sh
sudo apt -y install libpcl-dev
sudo apt install ros-noetic-pcl-conversions
```

修改cpp中node_vis.header.frame_id = "world";

rqt_graph安装

```sh
sudo apt-get install ros-noetic-rqt-graph
```


