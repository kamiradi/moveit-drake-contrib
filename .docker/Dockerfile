FROM robotlocomotion/drake:1.29.0

ENV ROS_DISTRO=rolling
ENV MOVEIT_UNDERLAY=/root/moveit/

# ROS installation
RUN apt update && \
    apt install locales && \
    locale-gen en_US en_US.UTF-8 && \
    update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8 && \
    export LANG=en_US.UTF-8

RUN apt install -y software-properties-common && \
    add-apt-repository universe

RUN apt update && \
    apt install -y curl git  && \
    curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg && \
    echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null

RUN apt-get update && \
    apt-get upgrade -y && \ 
    apt-get install -y ros-${ROS_DISTRO}-desktop && \
    apt-get install -y python3-rosdep && \
    apt-get install -y python3-vcstool && \
    apt-get install -y python3-colcon-common-extensions wget && \
    apt install -y ros-dev-tools

RUN apt update && \
    rosdep init && \
    rosdep update

RUN . /opt/ros/${ROS_DISTRO}/setup.sh && \
    apt remove ros-$ROS_DISTRO-moveit*

# create entrypoint
COPY .docker/ros_entrypoint.sh /sbin/ros_entrypoint.sh
RUN sudo chmod 755 /sbin/ros_entrypoint.sh
ENTRYPOINT ["/sbin/ros_entrypoint.sh"]
