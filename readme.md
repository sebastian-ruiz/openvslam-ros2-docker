# Openvslam ROS 2 in Docker

Aim: to install openvslam for ROS 2 with reproducibility.

This uses this [openvslam ros2 fork by Mirella Melo](https://github.com/mirellameelo/openvslam/tree/ros2-base#personal-notes-for-running-it-using-ros2).

## Installation

1. Run container:
```
docker-compose up -d

```
2. Once built, sh into container:
```
docker exec -ti ros2-dashing /bin/bash

```
3. Run the commands from [Mirella Melo](https://github.com/mirellameelo/openvslam/tree/ros2-base#personal-notes-for-running-it-using-ros2):
```
# Clone the repository
cd $HOME
git clone -b ros2-base --single-branch https://github.com/mirellameelo/openvslam.git

#Build Vision openCV and Image Common packages
source /opt/ros/dashing/setup.bash
cd $HOME/openvslam/ros2
colcon build --packages-select cv_bridge camera_calibration_parsers image_geometry image_transport opencv_tests vision_opencv camera_info_manager

# Source Vision openCV and Image Common packages
source $HOME/openvslam/ros2/install/setup.bash

# Build openvslam project
cd $HOME/openvslam
mkdir build && cd build
cmake     -DBUILD_WITH_MARCH_NATIVE=OFF     -DUSE_PANGOLIN_VIEWER=ON     -DUSE_SOCKET_PUBLISHER=OFF     -DUSE_STACK_TRACE_LOGGER=ON     -DBOW_FRAMEWORK=DBoW2     -DBUILD_TESTS=ON     ..
make -j4
sudo make install

# Build openvslam packages
cd $HOME/openvslam/ros2
colcon build --symlink-install
```


To rebuild image run:
```
docker-compose build
```
