# ubuntu20.04-ros-noetic-px4

环境配置

ubuntu20.04-ros-noetic-px4

[dockerfile](https://github.com/osrf/docker_images/blob/df19ab7d5993d3b78a908362cdcd1479a8e78b35/ros/noetic/ubuntu/focal/ros-base/Dockerfile)

在docker内配置走代理

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
--gpus all \
--env="DISPLAY" \
--env="QT_X11_NO_MITSHM=1" \
--volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
ros:noetic

sudo apt-get update
sudo apt-get install vim -y
```

[换源](https://www.yisu.com/ask/4042.html)

[PX4安装](https://docs.px4.io/main/en/dev_setup/dev_env_linux_ubuntu.html#ros-gazebo-classic)

```sh
sudo apt update
sudo apt install git -y
git clone https://github.com/PX4/PX4-Autopilot.git --recursive
```
```sh
bash ./PX4-Autopilot/Tools/setup/ubuntu.sh
```

https://github.com/PX4/PX4-Autopilot/blob/main/Tools/setup/ubuntu.sh

重启docker

```sh
sudo apt-get install protobuf-compiler libeigen3-dev libopencv-dev -y
```

[mavros安装](https://docs.px4.io/main/en/ros/mavros_installation.html)

```sh
sudo apt-get install ros-noetic-mavros ros-noetic-mavros-extras ros-noetic-mavros-msgs -y
wget https://raw.githubusercontent.com/mavlink/mavros/master/mavros/scripts/install_geographiclib_datasets.sh
sudo bash ./install_geographiclib_datasets.sh   
```

rviz安装

```sh
sudo apt-get install ros-noetic-rviz -y
```

pcl安装

```sh
sudo apt -y install libpcl-dev
sudo apt install ros-noetic-pcl-conversions -y
```

修改cpp中node_vis.header.frame_id = "world";

rqt_graph安装

```sh
sudo apt-get install ros-noetic-rqt-graph -y
```

docker支持gpu需要以下配置：

注意：docker run 需要添加参数 `--gpus all`

容器内可以读显卡
```sh
nvidia-smi
```

[CUDA Toolkit安装 只装Base Installer CUDA 工具包包含创建、构建和运行 CUDA 应用程序所需的 CUDA 驱动程序和工具以及库、头文件和其他资源](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=20.04&target_type=deb_local)

验证CUDA是否安装成功
```sh
/usr/local/cuda/bin/nvcc --version
```
vim ~/.bashrc添加CUDA进环境
```sh
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-12/lib64
export CUDA_HOME=/usr/local/cuda-12
export PATH=$PATH:/usr/local/cuda-12/bin
```
[CUDA安装](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/)

[conatiner-toolkit安装？这个不知道装了有什么用](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)


安装PaddlePaddle的gpu版本（CUDA10.2）

```sh
sudo apt-get update
sudo apt-get install git -y
sudo apt-get install python3-pip -y
sudo apt-get install ubuntu-drivers-common -y
python3 -m pip install paddlepaddle-gpu==2.3.2 -i https://mirror.baidu.com/pypi/simple
```
```sh
python3 -c "import paddle; print(paddle.__version__)"
```
安装Paddledection

```sh
# Clone PaddleDetection repository
cd <path/to/clone/PaddleDetection>
git clone https://github.com/PaddlePaddle/PaddleDetection.git

# Install other dependencies
cd PaddleDetection
pip install -r requirements.txt

# Compile and install paddledet
python3 setup.py install

sudo apt-get update
sudo apt-get install libgl1-mesa-glx
pip install numba==0.56.4
sudo apt-get install wget

# After installation, make sure the tests pass:
python3 ppdet/modeling/tests/test_architectures.py
```





