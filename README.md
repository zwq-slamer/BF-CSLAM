# Bandwidth-Efficient Collaborative SLAM: Mutual Information for Data Selection and Finite-State Entropy for Lossless Compression
## Introduction

This repository contains an implementation of our **bandwidth-optimized collaborative multi-robot SLAM method**, **integrated into the [COVINS](https://github.com/VIS4ROB/COVINS)** framework. In scenarios where **resource-constrained robots** collaborate with a **central server**, limited communication bandwidth often becomes a **critical bottleneck** for real-time mapping and localization.

To address this challenge, we propose a **mutual-information-based keyframe selection strategy** that transmits only the **most informative** visual keyframes, **significantly reducing redundant data**. Furthermore, we apply **finite-state entropy coding** for **lossless compression**, further minimizing transmission costs.

Since this work is **integrated into COVINS**, it can be **easily adopted** by users familiar with COVINS and **seamlessly incorporated into existing SLAM pipelines**, enabling **better scalability** and **reduced wireless communication load** in practical deployments.
![Syatem overview](docs/Fig1.png)

## Installaton
The current running environment is Ubuntu 20.04,[ROS noetic](https://example.com),OpcnCV 4.2.
### Back-end
The dependencies of the project can refer to [COVINS](https://github.com/VIS4ROB-lab/covins.git).
```
sudo apt-get update
sudo apt-get install libpthread-stubs0-dev build-essential cmake git doxygen libsuitesparse-dev libyaml-cpp-dev libvtk6-dev python3-wstool libomp-dev libglew-dev
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros-latest.list'
wget http://packages.ros.org/ros.key -O - | sudo apt-key add -
sudo apt-get update
sudo apt-get install python3-catkin-tools
```
Create the workspace
```
mkdir -p catkin_ws/src
cd catkin_ws
catkin init
catkin config --extend /opt/ros/noetic/
catkin config --merge-devel
catkin config --cmake-args -DCMAKE_BUILD_TYPE=RelWithDebInfo
```
Note that this project is quite strict about the versions of dependencies used. If the installation fails, please carefully refer to the versions corresponding to your platform. If the installation fails, clear the cache and then try installing again.
```
cd catkin_ws/src
git clone 
```











