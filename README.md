# MIT VNAV Lab 3 - Quadrotor Geometric Controller

## Overview

This repository contains my implementation for MIT VNAV Lab 3. The project implements a geometric quadrotor controller in ROS and evaluates it in the Unity/TESSE simulation environment.

The controller subscribes to the desired trajectory and simulated odometry, computes the required collective thrust and body moments, converts them into four rotor-speed commands, and publishes the commands back to the simulator.

## Implemented Functions

- ROS subscribers for the desired state and current odometry
- Rotor-speed command publisher
- Position and velocity error calculation
- Desired attitude construction from the commanded force and yaw
- Geometric attitude control on SO(3)
- Collective-thrust and body-moment calculation
- Wrench-to-rotor allocation
- Stationary hover testing at a target height of 2.0 m
- Circular-trajectory tracking in RViz and Unity/TESSE

## Source Structure

```text
controller_pkg/
├── CMakeLists.txt
├── package.xml
├── launch/
│   └── traj_tracking.launch
├── rviz/
│   └── lab3.rviz
└── src/
    ├── controller_node.cpp
    └── traj_publisher.cpp
```

The main implementation is located in `controller_pkg/src/controller_node.cpp`. The supplied trajectory generator is located in `controller_pkg/src/traj_publisher.cpp`.

## Software Environment

- Ubuntu 20.04
- ROS Noetic
- C++11
- Eigen3
- Unity/TESSE simulator
- TESSE ROS bridge
- RViz
- catkin tools

## Build

Place `controller_pkg` in the `src` directory of the VNAV catkin workspace, then run:

```bash
cd ~/vnav_ws
catkin build
source devel/setup.bash
```

The package depends on `roscpp`, `rospy`, `std_msgs`, `geometry_msgs`, `mav_msgs`, `nav_msgs`, `trajectory_msgs`, `tf`, `tf2`, `tf2_ros`, `tf2_geometry_msgs`, `tf2_eigen`, and Eigen3.

## Run

Start the ROS master, Unity/TESSE simulator, and TESSE ROS bridge before launching the controller:

```bash
roslaunch controller_pkg traj_tracking.launch
```

The launch file starts both `traj_publisher` and `controller_node` with the following controller gains:

```text
kx     = 1.0
kv     = 2.0
kr     = 2.0
komega = 1.0
```

The controller uses the following ROS topics:

| Direction | Topic | Message type |
| --- | --- | --- |
| Subscribe | `/desired_state` | `trajectory_msgs/MultiDOFJointTrajectoryPoint` |
| Subscribe | `/current_state` | `nav_msgs/Odometry` |
| Publish | `/rotor_speed_cmds` | `mav_msgs/Actuators` |

## Demonstration Video

The demonstration video link can be supplied separately when required by the submission instructions.

## Notes

- The commanded hover height used in the final experiment was 2.0 m.
- This README is project documentation and does not replace the required source-code submission.

