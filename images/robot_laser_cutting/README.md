# Base Docker images for the Robotic Laser Cutting demos

|                | demo1_drawing_task_camera_system | demo1_drawing_task_controller | demo1_drawing_task_coppeliasim    |
| :------------: | :-------------------------------: | :-------------------------: | :--------------------------------: |
| **Base Image** | `ubuntu:22.04`                    | `murilomarinho/sas:jazzy`   | `murilomarinho/coppeliasim:latest` |
| **External Libraries Installed** | [ROS 2 Humble](https://docs.ros.org/en/humble/index.html) \ [OpenCV (Python)](https://opencv.org/) \ [pyrealsense2](https://pypi.org/project/pyrealsense2/) \ [perception-tools](https://github.com/Adorno-Lab/perception-tools) | [robot_constraint_editor](https://github.com/Adorno-Lab/robot_constraint_editor) \ [robot_constraint_manager](https://github.com/Adorno-Lab/robot_constraint_manager) | - |
