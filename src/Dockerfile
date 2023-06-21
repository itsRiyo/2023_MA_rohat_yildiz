FROM ros:noetic-ros-core-focal

ENV WS_DIR="/ros_ws"


RUN apt-get update \
 && apt-get install -y \
    build-essential \
    cmake \
    git-all \
    software-properties-common \
 && rm -rf /var/lib/apt/lists/*
 
 
RUN apt-get update \
 && apt-key adv --keyserver keyserver.ubuntu.com --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE || apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE \
 && add-apt-repository "deb https://librealsense.intel.com/Debian/apt-repo $(lsb_release -cs) main" -u \
 && apt-get install -y \
    librealsense2-dkms \
    librealsense2-utils \
    librealsense2-dev \
    librealsense2-dbg 
    
RUN /bin/bash -c 'source /opt/ros/noetic/setup.bash &&\
    mkdir -p ~/catkin_ws/src &&\
    cd ~/catkin_ws/src &&\
    #catkin_init_workspace &&\
    cd ~/catkin_ws &&\
    catkin_make'
 
RUN apt-get install -y python3 pip \
 && pip install -U rosdep \
 && rosdep init \
 && rosdep update \
 && /bin/bash -c 'cd ~/catkin_ws &&\
    rosdep install -y -r --from-paths src/'
 
RUN apt install -y ros-noetic-realsense2-camera

RUN /bin/bash -c 'source /opt/ros/noetic/setup.bash &&\
    mkdir -p ~/catkin_ws/src &&\
    cd ~/catkin_ws/src &&\
    git clone https://github.com/IntelRealSense/realsense-ros &&\
    cd realsense-ros &&\
    git checkout ros1-legacy &&\
    cd ../.. &&\
    #catkin_init_workspace &&\
    cd ~/catkin_ws &&\
    catkin_make -DCATKIN_ENABLE_TESTING=False -DCMAKE_BUILD_TYPE=Release'

RUN echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
