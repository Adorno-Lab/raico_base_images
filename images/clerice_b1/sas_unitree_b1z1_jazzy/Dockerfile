# Base image for clerice_b1 / sas_unitree_b1z1_jazzy.
# SAS drivers for the Unitree B1 quadruped and Z1 arm, the B1 EKF, and the
# sas_unitree_b1z1_control_template package, on ROS 2 Jazzy.
# Moved from https://github.com/Adorno-Lab/sas_unitree_b1z1_control_template/tree/main/devel/sas_unitree_b1z1_jazzy
# Per RAICo's Docker standards, SAS-dependent containers must derive from
# murilomarinho/sas:jazzy.
# https://github.com/Adorno-Lab/development-guidelines/tree/main/3-docker-standards

FROM murilomarinho/sas:jazzy
SHELL ["/bin/bash", "-c"]
ENV BASH_ENV="/etc/bash_env"

LABEL org.opencontainers.image.source="https://github.com/Adorno-Lab/raico_base_images"
LABEL org.opencontainers.image.description="Base image for clerice_b1 / sas_unitree_b1z1_jazzy"
LABEL org.opencontainers.image.licenses="LGPL-2.1-only"



########################################################################################################
# Update SAS
# Upgrade only the SAS packages already in murilomarinho/sas:jazzy. Installing the
# ros-jazzy-sas-* wildcard also tries to install every other SAS package (e.g.
# sas-robot-driver-gazebo, whose dependencies are not installable), and the
# -dbgsym packages pin older versions of the packages they belong to.
RUN apt-get update && \
    apt-get purge -y 'ros-jazzy-*-dbgsym' && \
    apt-get install --only-upgrade -y 'ros-jazzy-sas-*'



########################################################################################################
# Create the folder /app if not present
WORKDIR /app


########################################################################################################
# Create the folder /app/software/ROS2/ros2_ws/src if not present
RUN mkdir -p /app/software/ROS2/ros2_ws/src




########################################################################################################

# Clone the sas_tools repository
RUN cd /app/software/ROS2/ros2_ws/src && git clone https://github.com/Adorno-Lab/sas_tools --recursive

# Clone the sas_robot_driver_unitree_z1
RUN cd /app/software/ROS2/ros2_ws/src && git clone https://github.com/Adorno-Lab/sas_robot_driver_unitree_z1 --recursive

# Clone the sas_robot_driver_unitree_b1
RUN cd /app/software/ROS2/ros2_ws/src && git clone https://github.com/Adorno-Lab/sas_robot_driver_unitree_b1 --recursive

# Set environment variables properly using ENV
ENV LD_LIBRARY_PATH="${LD_LIBRARY_PATH}:/app/software/ROS2/ros2_ws/src/sas_robot_driver_unitree_z1/src/z1_sdk/lib/:/app/software/ROS2/ros2_ws/src/sas_robot_driver_unitree_b1/src/unitree_legged_sdk/lib/cpp/amd64/"
ENV LIBRARY_PATH="${LIBRARY_PATH}:/app/software/ROS2/ros2_ws/src/sas_robot_driver_unitree_z1/src/z1_sdk/lib/:/app/software/ROS2/ros2_ws/src/sas_robot_driver_unitree_b1/src/unitree_legged_sdk/lib/cpp/amd64/"
##########################################################################################



# Create the git folder to clone github repositories if not present.
#######################################################################################################
RUN mkdir -p ~/git



# Clone the rap2025_wholebodycontrol and copy the EKF package 
#######################################################################################################
RUN cd ~/git && git clone https://github.com/Adorno-Lab/rap2025_wholebodycontrol.git --recursive
RUN cp -r ~/git/rap2025_wholebodycontrol/software/ROS2/ros2_ws/src/sas_extended_kalman_filter_unitree_b1 /app/software/ROS2/ros2_ws/src



# Clone the sas_unitree_b1z1_control_template and copy the template package and the utils folder.
# (This image is built from raico_base_images, so the files cannot be COPY'd from that repository.)
#######################################################################################################
WORKDIR /app

RUN cd ~/git && git clone https://github.com/Adorno-Lab/sas_unitree_b1z1_control_template && \
    mkdir -p /app/software/ROS2/ros2_ws/src && \
    cp -r ~/git/sas_unitree_b1z1_control_template/ros2_ws/src/sas_unitree_b1z1_control_template /app/software/ROS2/ros2_ws/src/ && \
    cp -r ~/git/sas_unitree_b1z1_control_template/utils /app/software/ROS2/utils


##########################################################################################
# Install Unitree kinematics

RUN cd ~/git && git clone https://github.com/Adorno-Lab/unitree_kinematics.git
RUN cd ~/git/unitree_kinematics && mkdir -p build && cd build && cmake .. && make && make install



# Add some useful shortcuts
RUN echo "source /opt/ros/jazzy/setup.bash" >> /etc/bash_env
RUN echo "# Source the ROS2 overlay, as instructed in https://github.com/SmartArmStack/smart_arm_stack_ROS2" >> /etc/bash_env
RUN echo "alias cdros2='cd /app/software/ROS2/ros2_ws/' " >> /etc/bash_env
RUN echo "alias buildros2='cdros2 && colcon build && source install/setup.bash' " >> /etc/bash_env

# Build the ROS2 packages
RUN bash -c "source /opt/ros/jazzy/setup.bash; cd /app/software/ROS2/ros2_ws/; colcon build && source install/setup.bash" 

RUN echo "source /etc/bash_env" >> ~/.bashrc
