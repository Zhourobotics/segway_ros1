# Segway Robot ROS1 Package

This is a docker container that allows us to develop controller for the Segway robot. 
The following is the summary taken from the package given to us by Segway Robotics. 

## 1 Chassis PC Motion Test

(1) Turn off the remote control; or turn on the remote control, at the same time, enable the lever to move upwards to the PC mode, and make sure that the emergency stop lever is not in the emergency stop state by moving downwards.

(2) Enter the ROS workspace and run the following command to compile the segway_msgs and segwayrmp packages.

```
cd catkin_ws
catkin_make -DCATKIN_WHITELIST_PACKAGES='segway_msgs' && catkin_make -DCATKIN_WHITELIST_PACKAGES='segway_rmp'
```
and then run the following after sourcing `devel/setup.bash` and running `roscore`

- `rosrun segwayrmp SmartCar`
- `rosrun segwayrmp drive_segway_sample`



## 2 Main File Path Description

(1) segway_msgs: Custom message package

`segway_msgs/msg`: Custom message

(2) segwayrmp: Package where the SmartCar node is located

`segwayrmp/include`: Path of the header files required by the node

`segwayrmp/lib`: Path where the dynamic library required by ROS for the PC is located

`segwayrmp/src`: SmartCar node source file program for interacting with the chassis data

`segwayrmp/tools`: Source program for the drive_segway_sample node used for motion testing

Documentation for the Docker side of things is mentioned below. All thanks to the original contributers for this isolated ROS 1 based Docker Environment. 
_______________________________________________________________________________________________________________________________________

## ROS in Docker

Tired of trying to compile ROS1 on Ubuntu 22.04? Tired of breaking your systems due to conflicts with
the ton of dependencies ROS1 has? Here we propose a solution. A completely isolated environment,
with ROS1 support by default, extensible, and with all the tools included for developing ROS1
applications without having to install them locally.

https://user-images.githubusercontent.com/21349875/199551739-c33dee77-57c7-4c10-a272-c60b98645368.mp4

## Tested host-machines

- Ubuntu 22.04

## Supported development environments

- [x] tmux
- [x] VSCode

## How to use with your project (VSCode)
![vscode](https://user-images.githubusercontent.com/21349875/200361817-572292e3-3d73-4fb1-bd9d-562539fa2fb4.png)

1. Click on "Use this template", or fork if you like.
1. Git clone the repo locally and `cd` into it.
1. Create an `src/` directory inside the repository: `mkdir -p src/`
1. Clone inside the `src/` directory the ROS1 code you want to
 develop/test. I will be using the
   [ros_tutorials](https://github.com/ros/ros_tutorials) as an example, but it can be as complex as
   you wish, so, `git clone git@github.com:ros/ros_tutorials.git src/`
1. Launch `code .`, and then go to the "Remote Explorer" tab and hit "reopen the current folder in a
   container", this should launch a full dev environemnt with some extensions to develop your ROS
   application in the dockerize environment
1. Launch the `Build Task`, Ctrl+Shift+p and type "Tasks: Run Build Task"

## How to use with your project (tmux)

![tmux](https://user-images.githubusercontent.com/21349875/200361914-446b13a8-ee50-436b-be9f-0bbb8f48ce43.png)

1. Click on "Use this template", or fork if you like.
1. Git clone the repo locally and `cd` into it.
1. Launch `make docker` to create a local docker container image with all the nice stuff installed.
   If you need any extra dependency, now is the time to add it to the [Dockerfile](./Dockerfile).
1. Create an `src/` directory inside the repository: `mkdir -p src/`
1. Clone inside the `src/` directory the ROS1 code you want to develop/test. I will be using the
   [ros_tutorials](https://github.com/ros/ros_tutorials) as an example, but it can be as complex as
   you wish, so, `git clone git@github.com:ros/ros_tutorials.git src/`
1. Launch `make`, this will build your project and open a `tmux` session with all the batteries
   included.

## What about injecting rosbags inside the dev container?

Got you covered, just use the environment variable `export ROS_BAGS=/path/to/data/in/host` and
launch `make`. Your rosbag files will be mounted in the dev container in `~/ros_ws/bags` and are
read-only accessible to the ROS1 applications.

## How to extend the container

Could not be more simple, just add all your user-space command on the [Dockerfile](./Dockerfile).
`apt install <your-libs>` and enjoy the setup.

## GUI Supported?

Of course, you can run `rviz` and friends inside the dev container, it looks a bit ugly but at least
it works.

## Install host-machine dependencies

For now, you only need, I expect this repo to be used by "intermediate" developers, so I guess you
can figure out how to do that.

- docker
- docker-compose
- (VSCode) dev-containers extension

## Security concerns

The entire setup is NOT safe at all, so, use it at your own risk. I'm mounting directories from the
host machine to the docker container where the user has `sudo` access without a password. So you can
literally delete some stuff with `root` permissions without even typing a password. So, you are
warned!

## About the tools for development

I built this project on top of my [dotfiles](https://github.com/nachovizzo/dotfiles/blob/main/.config/yadm/bootstrap),
so it's completely overfitted to my own needs and I don't intend to provide support for extra stuff.
If there is something you don't like or don't need, feel free to modify your copy of the Dockerfile.
You are in full control of what you want and not.
