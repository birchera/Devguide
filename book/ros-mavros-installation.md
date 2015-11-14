
# MAVROS
## Installation
### Binary installation (debian)

Since v0.5 that programs available in precompiled debian packages for x86 and amd64 (x86\_64).
Also v0.9+ exists in ARMv7 repo for Ubuntu armhf.
Just use `apt-get` for installation:
```sh
$ sudo apt-get install ros-indigo-mavros ros-indigo-mavros-extras
```

### Source installation
**Dependencies**

This installation assumes you have a catkin workspace located at `~/catkin_ws` If you don't create one with: 
```sh
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws
$ catkin init
```

You will be using the ROS python tools `wstool, rosinstall,and catkin_tools` for this installation. While they may have been installed during your installation of ROS you can also install them with:
```sh
$ sudo apt-get install python-wstool python-rosinstall-generator python-catkin-tools
```

Note that while the package can be built using catkin_make the prefered method is using catkin_tools as it is a more versitile and "friendly" build tool.

If this is your first time using wstool you will need to initialize your source space with:
```sh
$ wstool init ~/catkin_ws/src
```

Now you are ready to do the build
```sh
    # 1. get source (upstream - released)
$ rosinstall_generator --upstream mavros | tee /tmp/mavros.rosinstall
    # alternative: latest source
$ rosinstall_generator --upstream-development mavros | tee /tmp/mavros.rosinstall

    # 2. get latest released mavlink package
    # you may run from this line to update ros-*-mavlink package
$ rosinstall_generator mavlink | tee -a /tmp/mavros.rosinstall

    # 3. Setup workspace & install deps
$ wstool merge -t src /tmp/mavros.rosinstall
$ wstool update -t src
$ rosdep install --from-paths src --ignore-src --rosdistro indigo -y

    # finally - build
$ catkin build
```

*Important*. The current implementation of mavlink does not allow to select dialect in run-time,
so mavros package (and all plugin packages) have compile-time option `MAVLINK_DIALECT`, default is 'aurdupilotmega'.

If you want change dialect change workspace config:
```sh
$ catkin config --cmake-args -DMAVLINK_DIALECT=pixhawk
```