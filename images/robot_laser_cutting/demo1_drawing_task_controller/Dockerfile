FROM murilomarinho/sas:jazzy
ENV BASH_ENV="/etc/bash_env"

# Update apt-get
RUN apt-get update -q \
    && apt-get autoremove -y \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Create a folder for GitHub pulls
RUN mkdir -p ~/git/

# Install robot_constraint_editor
RUN cd ~/git && git clone https://github.com/Adorno-Lab/robot_constraint_editor
RUN cd ~/git/robot_constraint_editor && git checkout ec72aa22771a58d77ca6f3f09a0420e52cb605ee

RUN cd ~/git/robot_constraint_editor && mkdir -p build && cd build && cmake .. && make && make install

# Install the Robot Constraint Manager
RUN cd ~/git && git clone https://github.com/Adorno-Lab/robot_constraint_manager
RUN cd ~/git/robot_constraint_manager && git checkout 2a722d8893777c1c39f0ff54a342733b652e6917

RUN mkdir -p ~/git/robot_constraint_manager/build
RUN cd ~/git/robot_constraint_manager/build && cmake .. && make && make install
RUN ldconfig