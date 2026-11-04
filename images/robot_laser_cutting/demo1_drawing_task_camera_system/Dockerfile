FROM murilomarinho/sas:jazzy
ENV BASH_ENV="/etc/bash_env"
ARG DEBIAN_FRONTEND=noninteractive # to avoid geographical questions when trying to install ROS

# Update apt-get
RUN apt-get update -q \
    && apt-get autoremove -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Downgrade numpy
RUN pip install numpy==1.26.4 pyyaml --break-system-packages

# Install OpenCV
RUN apt-get update && apt-get install -y python3-pip python3-venv
RUN apt-get install -y python3-pip
RUN pip install opencv-python --break-system-packages && pip install opencv-contrib-python --break-system-packages

# Install pyrealsense2
RUN pip install pyrealsense2 --break-system-packages
