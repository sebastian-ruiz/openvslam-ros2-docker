FROM osrf/ros:dashing-desktop
MAINTAINER Sebastian Ruiz

WORKDIR /root

# install some extra dependencies
RUN apt-get update
RUN apt-get upgrade -y --no-install-recommends

# general dependencies
RUN apt-get install -y build-essential pkg-config cmake git wget curl unzip

# g2o dependencies
RUN apt-get install -y libatlas-base-dev libsuitesparse-dev

# Pangolin dependencies
RUN apt-get install -y libglew-dev

# OpenCV dependencies
RUN apt-get install -y libgtk-3-dev ffmpeg libavcodec-dev libavformat-dev libavutil-dev libswscale-dev libavresample-dev

# eigen dependencies
RUN apt-get install -y gfortran

# other dependencies
RUN apt-get install -y libyaml-cpp-dev libgoogle-glog-dev libgflags-dev

# add ros to bashrc
RUN echo 'source "/opt/ros/$ROS_DISTRO/setup.bash"' >> /root/.bashrc

# Install Open CV - Warning, this takes a while
RUN cd /root && \
    wget https://github.com/opencv/opencv/archive/4.4.0.zip && \
    unzip 4.4.0.zip && \
    rm 4.4.0.zip && \
    cd opencv-4.4.0 && \
    mkdir build && \
    cd build && \
    cmake -D CMAKE_BUILD_TYPE=RELEASE \
      -D CMAKE_INSTALL_PREFIX=/usr/local \
      -D BUILD_TIFF=ON \
      .. && \
    make -j4 && \
    make install

# Install g2o
RUN cd /root && \
    wget https://github.com/RainerKuemmerle/g2o/archive/20200410_git.zip && \
    unzip 20200410_git.zip && \
    rm 20200410_git.zip && \
    cd g2o-20200410_git && \
    mkdir build && \
    cd build && \
    cmake ../ && \
    make && \
    make install

# Install DBoW2
RUN cd /root && \
    git clone https://github.com/shinsumicco/DBoW2 && \
    cd DBoW2 && \
    mkdir build && \
    cd build && \
    cmake .. && \
    make && \
    make install

# Install Pangolin
RUN cd /root && \
    git clone https://github.com/stevenlovegrove/Pangolin.git && \
    cd Pangolin && \
    mkdir build && \
    cd build && \
    cmake .. && \
    make && \
    make install

# Install Openvslam for ROS2
# using https://github.com/mirellameelo/openvslam/tree/ros2-base
# RUN source /opt/ros/$ROS_DISTRO/setup.bash
# RUN cd /root && \
#     git clone -b ros2-base --single-branch https://github.com/mirellameelo/openvslam.git && \
#     cd openvslam/ros2 && \
#     colcon build --packages-select cv_bridge camera_calibration_parsers image_geometry image_transport opencv_tests vision_opencv camera_info_manager
# RUN source /root/openvslam/ros2/install/setup.bash
# RUN cd /root/openvslam && \
#     mkdir build && \
#     cd build && \
#     cmake -DBUILD_WITH_MARCH_NATIVE=OFF \
#         -DUSE_PANGOLIN_VIEWER=ON \
#         -DUSE_SOCKET_PUBLISHER=OFF \
#         -DUSE_STACK_TRACE_LOGGER=ON \
#         -DBOW_FRAMEWORK=DBoW2 \
#         -DBUILD_TESTS=ON .. && \
#     make && \
#     make install && \
#     cd /root/openvslam/ros2

# Instructions say to do this:
#    colcon build --symlink-install
# but it fails :(

