# docker-ur5-ros

This repository contains a docker image for running ur5

## How to pull

Open a terminal and run the following command
```console
docker pull ghcr.io/olivianocentini/docker-ur5-ros:latest
```

## How to create a container
This command is required to create a container (only once)
```console
xhost +
docker run -it \
           --volume=/tmp/.X11-unix:/tmp/.X11-unix \
           --device=/dev/dri:/dev/dri \
           --env="DISPLAY=$DISPLAY" \
           --name=ur5-ros-container \
           ghcr.io/olivianocentini/docker-ur5-ros 
```
This will also open a bash session.


## How to start a container
Every time you want to use the container you should run (at beginning) the following command
```console
docker start ur5-ros-container
```

## How to start a terminal
Once the container is running (see the previous section) you can start a terminal with the following command
```console
docker exec -it ur5-ros-container bash
```

You can finally follow this guide to enable the realtime control in UR5 https://ros-planning.github.io/moveit_tutorials/doc/realtime_servo/realtime_servo_tutorial.html

