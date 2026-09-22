FROM murilomarinho/sas:jazzy
ENV BASH_ENV="/etc/bash_env"

# Update apt-get
RUN apt-get update -q \
    && apt-get upgrade -y \
    && apt-get autoremove -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Create a folder for GitHub pulls
RUN mkdir -p ~/git/

# Install robot_constraint_editor
RUN cd ~/git && git clone https://github.com/Adorno-Lab/robot_constraint_editor
RUN cd ~/git/robot_constraint_editor && mkdir -p build && cd build && cmake .. && make && make install

# Install the Robot Constraint Manager
RUN cd ~/git && git clone https://github.com/Adorno-Lab/robot_constraint_manager
RUN cd ~/git/robot_constraint_manager && git pull

RUN mkdir -p ~/git/robot_constraint_manager/build
RUN cd ~/git/robot_constraint_manager/build && cmake .. && make && make install
RUN ldconfig