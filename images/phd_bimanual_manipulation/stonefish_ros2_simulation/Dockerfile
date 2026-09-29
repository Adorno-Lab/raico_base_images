# Stage 1: Builder

FROM ros:jazzy AS builder

ARG DEBIAN_FRONTEND=noninteractive

# Use bash as the default shell
SHELL ["/bin/bash", "-c"]


# Install dependencies required to install the Stonefish Library
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        libglm-dev \
        libsdl2-dev \
        libfreetype6-dev \
        libgl1-mesa-dri \
        && rm -rf /var/lib/apt/lists/*


# Clone specific commit
RUN git clone git://github.com/patrykcieslak/stonefish.git /root/stonefish && \
    cd /root/stonefish && \
    git checkout b21eb8e194c570ff2f61e91aeffb38d73dc25f42


# Build and install Stonefish
WORKDIR /root/stonefish/build

RUN cmake .. && \
    make -j4 && \
    make install


# Stage 2: Development

FROM ros:jazzy

ARG DEBIAN_FRONTEND=noninteractive

# Use bash as the default shell
SHELL ["/bin/bash", "-c"]

# Install runtime dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        libglm-dev \
        libsdl2-dev \
        libfreetype6-dev \
        libgl1-mesa-dri \
        ros-jazzy-pcl-conversions \
        ros-jazzy-image-transport \
        && rm -rf /var/lib/apt/lists/*


# Copy the installed Stonefish Library over from previous stage
COPY --from=builder /usr/local/ /usr/local/

RUN ldconfig

# Create ROS2 folder
WORKDIR /root/stonefish_ros2_ws/src


# Clone Stonefish Ros2 with specific commit
RUN git clone \
        git://github.com/patrykcieslak/stonefish_ros2.git \
        stonefish_ros2 && \
    cd stonefish_ros2 && \
    git checkout 6646e7ac25eed982f37f807abc7b545dbd2d6648

# Create aliases
RUN echo "source /opt/ros/jazzy/setup.bash" >> /root/.bashrc && \
    echo "alias cdr2='cd /root/stonefish_ros2_ws'" >> /root/.bashrc && \
    echo "alias bsr2='cdr2 && colcon build && source install/setup.bash'" >> /root/.bashrc && \
    echo "alias cr2='cdr2 && rm -rf build install log'" >> /root/.bashrc

# Start in the ROS 2 workspace
WORKDIR /root/stonefish_ros2_ws
