FROM ubuntu:22.04
ENV BASH_ENV="/etc/bash_env"
ARG DEBIAN_FRONTEND=noninteractive # to avoid geographical questions when trying to install ROS

# Install DQ Robotics
RUN apt-get update && apt-get install -y libeigen3-dev libzmq3-dev libboost-all-dev python3-pybind11 python3-pip
RUN python3 -m pip install --user dqrobotics --upgrade

RUN ln -s /usr/include/eigen3/Eigen /usr/include/Eigen
RUN ldconfig

# Install ROS 2 Humble 
RUN apt-get update
RUN apt-get install -y software-properties-common curl
RUN add-apt-repository universe
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null

RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y ros-humble-desktop ros-dev-tools

# Create work folder
RUN mkdir -p ~/ros2_ws/src

# Create aliases
RUN echo "source /opt/ros/humble/setup.bash" >> /etc/bash_env
RUN echo "alias cdros2='cd ~/ros2_ws/' " >> /etc/bash_env
RUN echo "alias buildros2='cdros2 && colcon build && source install/setup.bash' " >> /etc/bash_env
RUN echo "source /etc/bash_env" >> ~/.bashrc

# Install OpenCV
RUN apt-get install -y python3-pip python3-venv
RUN apt-get install -y python3-pip
RUN pip install opencv-python && pip install opencv-contrib-python

# Install pyrealsense2
RUN pip install pyrealsense2

# Downgrade numpy
RUN pip install numpy==1.26.4 pyyaml