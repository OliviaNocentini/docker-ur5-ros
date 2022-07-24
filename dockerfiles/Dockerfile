FROM ros:melodic-ros-base-bionic
LABEL org.opencontainers.image.title="UR5-ros-melodic"
LABEL org.opencontainers.image.description="A ros image to enable realtime control via moveIt! on UR5"
LABEL org.opencontainers.image.source="https://raw.githubusercontent.com/OliviaNocentini/docker-ur5-ros/main/dockerfiles/Dockerfile"
LABEL org.opencontainers.image.authors="Olivia Nocentini <o.nocentini@gmail.com>"

ARG USERNAME=user
ARG USERID=1000

SHELL ["/bin/bash", "-c"]

# Non-interactive installation mode
ENV DEBIAN_FRONTEND=noninteractive

# Update apt database
RUN apt update

# Set the locale
RUN apt install -y -qq apt-utils locales && rm -rf /var/lib/apt/lists/*
RUN locale-gen en_US en_US.UTF-8
RUN update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
ENV LANG=en_US.UTF-8

# install other ros deps
RUN apt update && \
    apt install -y ros-melodic-urdf ros-melodic-moveit ros-melodic-moveit-servo && \
    rm -rf /var/lib/apt/lists/*

# Create the user
RUN useradd --create-home -s /bin/bash --no-user-group -u $USERID $USERNAME && \
    adduser $USERNAME sudo && \
    echo "$USERNAME ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers

USER $USERNAME
WORKDIR /home/$USERNAME

# Install ur5 related software
RUN mkdir -p ~/ws_moveit/src && \
    source /opt/ros/melodic/setup.bash && \
    cd ~/ws_moveit/src && \
    git clone -b melodic-devel https://github.com/ros-industrial/universal_robot && \
    sudo apt update && \
    cd ~/ws_moveit/src && \
    rosdep update && \
    rosdep install -y --from-paths . --ignore-src --rosdistro $ROS_DISTRO && \
    cd ~/ws_moveit/ && \
    catkin_make && \
    sudo rm -rf /var/lib/apt/lists/*

RUN echo "source /home/$USERNAME/ws_moveit/devel/setup.bash" >> /home/$USERNAME/.bashrc
