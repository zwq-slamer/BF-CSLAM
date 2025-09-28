# Bandwidth-Efficient Collaborative SLAM: Mutual Information for Data Selection and Finite-State Entropy for Lossless Compression
## Introduction

This repository contains an implementation of our **bandwidth-optimized collaborative multi-robot SLAM method**, **integrated into the [COVINS](https://github.com/VIS4ROB/COVINS)** framework. In scenarios where **resource-constrained robots** collaborate with a **central server**, limited communication bandwidth often becomes a **critical bottleneck** for real-time mapping and localization.

To address this challenge, we propose a **mutual-information-based keyframe selection strategy** that transmits only the **most informative** visual keyframes, **significantly reducing redundant data**. Furthermore, we apply **finite-state entropy coding** for **lossless compression**, further minimizing transmission costs.

Since this work is **integrated into COVINS**, it can be **easily adopted** by users familiar with COVINS and **seamlessly incorporated into existing SLAM pipelines**, enabling **better scalability** and **reduced wireless communication load** in practical deployments.
![Syatem overview](docs/Fig1.png)

## Installaton
The current running environment is Ubuntu 20.04,[ROS noetic](https://example.com),OpcnCV 4.2.
### Back-end
1. The dependencies of the backend can refer to [COVINS](https://github.com/VIS4ROB-lab/covins.git).
```
sudo apt-get update
sudo apt-get install libpthread-stubs0-dev build-essential cmake git doxygen libsuitesparse-dev libyaml-cpp-dev libvtk6-dev python3-wstool libomp-dev libglew-dev
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros-latest.list'
wget http://packages.ros.org/ros.key -O - | sudo apt-key add -
sudo apt-get update
sudo apt-get install python3-catkin-tools
```
2. Create the workspace
```
mkdir -p catkin_ws/src
cd catkin_ws
catkin init
catkin config --extend /opt/ros/noetic/
catkin config --merge-devel
catkin config --cmake-args -DCMAKE_BUILD_TYPE=RelWithDebInfo
```
3. Note that this project is quite strict about the versions of dependencies used. If the installation fails, please carefully refer to the versions corresponding to your platform. If the installation fails, clear the cache and then try installing again.
```
git clone https://github.com/zwq-slamer/BF-CSLAM.git
cd catkin_ws/src
cp -r ./backend/ ./catkin_ws/src/
chmod +x backend/install_file.sh
./src/backend/install_file.sh -f
git clone https://github.com/ros-perception/vision_opencv.git
```
4. Open `~/catkin_ws/src/vision_opencv/cv_bridge/CMakeLists.txt`, check the OpenCV version.
```
source ~/catkin_ws/devel/setup.bash
caktin build cv_bridge
```
### Front-end
1. Before installing the frontend, please make sure that the backend can run properly. The dependencies of the frontend can refer to [VINS_FUSION](https://github.com/HKUST-Aerial-Robotics/VINS-Fusion).
create the workspace
```
mkdir -p ~/vins_ws/src
cd ~/vins_ws && catkin init
catkin config --extend /opt/ros/noetic/
catkin config --cmake-args -DCMAKE_BUILD_TYPE=RelWithDebInfo
cd ~/vins_ws/src
cp -r ./frontend/ ./vins_ws/src/

```
2. Clone the following repositories as well：
```
git clone https://github.com/ethz-asl/ceres_catkin.git 
git clone https://github.com/catkin/catkin_simple.git 
git clone https://github.com/ethz-asl/opencv3_catkin.git 
git clone https://github.com/ethz-asl/eigen_catkin.git 
git clone https://github.com/ethz-asl/glog_catkin.git 

```
3. **Note** If running on Ubuntu 20 or above, modify the various CMakeLists.txt files in VINS.
`set(CMAKE_CXX_FLAGS "-std=c++17")` Because Ceres 2.0 and above are written using the C++17 standard, for more detailed issues regarding VINS_Fusion, you can find more detailed explanations in the VINS_FUSION [issues](https://github.com/HKUST-Aerial-Robotics/VINS-Fusion/issues).
```
cd ~/vins_ws && catkin build
```
## Configuration
In frontend vins folder `covins_comm/config/config_comm.yaml` change `sys.server_ip` to your IP.

## Run

In `config/euroc/euroc_mono_imu_config.yaml`,
```
roscore
#In COVINS folder
source ./devel/setup.bash
rosrun covins_backend covins_backend_node
#In VINS folder
source ./devel/setup.bash
#agent0
roslaunch vins vins_covins.launch config:=${PATH TO CONFIG file} ag_n:=0
#agent1
roslaunch vins vins_covins.launch config:=${PATH TO CONFIG file} ag_n:=1
# Play rosbag
rosbag play ~/EuRoC/MH_01_easy.bag /cam0/image_raw:=/cam0/image_raw0 /cam1/image_raw:=/cam1/image_raw0 /imu0:=/imu0
rosbag play ~/EuRoC/MH_02_easy.bag /cam0/image_raw:=/cam0/image_raw1 /cam1/image_raw:=/cam1/image_raw1 /imu0:=/imu1
# Visualization
roslaunch ~/catkin_ws/src/covins/covins_backend/launch/tf.launch
rviz -d ~/catkin_ws/src/covins/covins_backend/config/covins.rviz

```
## Acknowledgement
This project builds upon and is inspired by several excellent works:

- [COVINS](https://github.com/VIS4ROB-lab/covins.git).
- [VINS_FUSION](https://github.com/HKUST-Aerial-Robotics/VINS-Fusion).
- [LET-NET](https://github.com/linyicheng1/LET-NET)

We thank all the contributors of these open-source projects for their valuable work.





