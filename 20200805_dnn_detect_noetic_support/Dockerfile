# adapted from https://github.com/SteveMacenski/slam_toolbox/blob/noetic-devel/Dockerfile
FROM ros:noetic-ros-base-focal

# USE BASH
SHELL ["/bin/bash", "-c"]

# RUN LINE BELOW TO REMOVE debconf ERRORS (MUST RUN BEFORE ANY apt-get CALLS)
RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections

RUN apt-get update && apt-get upgrade -y && apt-get install -y --no-install-recommends \
    apt-utils \
    git

RUN mkdir -p catkin_ws/src
RUN cd catkin_ws/src && git clone https://github.com/UbiquityRobotics/dnn_detect.git
RUN source /opt/ros/noetic/setup.bash \
    && cd catkin_ws \
    && rosdep update \
    && rosdep install -y --from-paths src --ignore-src --rosdistro=noetic -y

RUN apt-get update && apt-get upgrade -y && apt-get install -y --no-install-recommends \
    python3-catkin-tools \
    python3-pip 
RUN pip3 install \
    osrf-pycommon 
RUN source /opt/ros/noetic/setup.bash \ 
    && cd catkin_ws/src \
    && catkin_init_workspace \
    && cd .. \
    && catkin config --install \
    && catkin build -DCMAKE_INSTALL_PREFIX=/opt/ros/noetic -DCMAKE_BUILD_TYPE=Release

#tests
# last step displays test output filtered as per https://github.com/catkin/catkin_tools/issues/405#issuecomment-573753780
RUN source /opt/ros/noetic/setup.bash \ 
    && cd catkin_ws/src \
    && set -o pipefail \
    && catkin run_tests | tee /tmp/test_results.txt \
    && cat /tmp/test_results.txt | sed -En '/^-- run_tests.py/,/^-- run_tests.py/p'

