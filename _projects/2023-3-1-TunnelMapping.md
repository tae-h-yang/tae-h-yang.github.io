---
title: "School Tunnel Mapping"
collection: projects
type: #"northeastern"
permalink: /projects/tunnel-mapping/ 
# date: 2023-5-8
period: Mar 2023 - May 2023
location: 
classes: wide
excerpt: "Created a map of school tunnel using various SLAM algorithms on sensor data collected by a LiDAR, Cameras, and IMU"
---

# Introduction

Various SLAM algorithms such as Hector SLAM, Gmapping, Cartographer, RTAB-Map and ORB-SLAM 3 were explored to create a map of school tunnel. Data of the school tunnel were collected by a LiDAR, Cameras, and IMU. The sensor data were stored as a rosbag format to test the algorithms individually by replaying the rosbag file on ROS. Each algorithm created either occupancy grid map or point cloud map and their performance was analyzed.

# Data Collection

<p style="text-align: center;"><img src="/assets/projects/School-Tunnel.png" width="250" height="250" /><strong><br />Route and map of school tunnel.</strong></p>
The school tunnel at Northeastern University looked like the map above. The red line in the map indicated where the sensor data were collected and the algorithms had to contruct a map. 

<p style="text-align: center;"><img src="/assets/projects/RPLiDAR-A3.png" width="150" height="150" /><img src="/assets/projects/Realsense-D435i.png" width="150" height="150" /><img src="/assets/projects/Realsense-T265.png" width="150" height="150" /><img src="/assets/projects/VectorNav-VN100.png" width="150" height="150" /><strong><br />RPLiDAR A3, Realsense D435i and T265, and VectorNav VN100 sensors from the left.</strong></p>

The sensors consisted of one LiDAR, two cameras, and one IMU. RPLiDAR A3 was a 2D planar LiDAR. Realsense D435i and T265 were a RGB-D and Stereo camera respectively. VectorNav VN100 was an IMU that contained a three-axis accelerometer, gyroscope, and magnetometer.

<p style="text-align: center;"><img src="/assets/projects/Sensor-Mount-Top.png" width="400" height="400" /><img src="/assets/projects/Sensor-Mount-Front.png" width="155" height="155" /><strong><br />Top side of the cart with the LiDAR and IMU and front side with the cameras.</strong></p>

The sensors were rigidly mounted on a rolling cart as the figures above to reduce noises as much as possible. The cart was pushed and traveled the tunnel following the trajectory planned in the beginning. During the sensor data collection, A laptop was reading all the data running ROS drivers for each sensor and recording them with a rosbag node.

<p style="text-align: center;"><img src="/assets/projects/Tunnel-Rosbag.png" width="400" height="400" /><strong><br />Rosbag file recorded.</strong></p>

# Hector SLAM
There was a ROS package implementing Hector SLAM which was a LiDAR based SLAM algorithm. Only the LiDAR sensor data was used and the localization and mapping was performed by scan match. Scan matching was finding the sensor pose that minimized the error between the map and the new scan data using the Gauss-Newton leaster squares approach. This meant that feature detection or mathcning wasn't required which reduced the computational load.

<p style="text-align: center;"><img src="/assets/projects/Hector-SLAM-1.png" width="400" height="400" /><img src="/assets/projects/Hector-SLAM-2.png" width="400" height="400" /><strong><br />Left: First 30 seconds of dataset showing good SLAM performance. Right: Entire map with some incorrect pose estimation.</strong></p>

Hector SLAM generated and expanded the map as the cart moved along and provided fair pose information. However, during the travel in long and straight sections of the tunnel, it wasn't able to function properly since there were no distinct differences in the consecutive LiDAR scans and the cart was assumed to be stationary.

# Gmapping SLAM
Another LiDAR based SLAM, Gmapping SLAM, was tested. It applied a Rao-Blackwellized particle filter for learning grid maps and created a 2D occupancy grid map from the laser scan and pose data. In ROS, the Gmapping SLAM node which was called `slam_gmapping` attempted to transform each incoming scan into the `odom` tf frame. 

Additional ROS node was required to provide accurate pose data to the Gmapping SLAM node. It was called `laser_scan_matcher_node` from the `laser_scan_matcher` package. This package was an incremental laser scan registration tool. It allowed to scan match between consecutive `sensor_msgs/LaserScan` messages and published the estimated pose of the sensor with a tf transform. Detailed instruction for running the Gmapping SLAM node can be found in [Appendix A](#appendix-a-gmapping-slam-in-ros).

<p style="text-align: center;"><img src="/assets/projects/Gmapping-SLAM.png" width="250" height="250" /><strong><br />Map generated from Gmapping SLAM.</strong></p>
The resulting map presented the same issue as Hector SLAM. When there were no distinctive differences between consecutive laser scan messages, the cart was assumed to stationary. Therefore, some tunnel areas were generated on top of the wrong locations and the entire map ended up having several overlapping areas.

# Cartographer SLAM
Cartographer SLAM used a combitionation of LiDAR and IMU data to construct 2D or 3D map of the environment while simultaneously estimating poses of the sensors. Detailed instruction for running the Cartographer SLAM node in ROS can be found in [Appendix B](#appendix-b-cartographer-slam-in-ros).

<p style="text-align: center;"><img src="/assets/projects/Cartographer-SLAM.png" width="300" height="300" /><strong><br />Map generated from Cartographer SLAM.</strong></p>

Since the IMU data were used, the long and straight sections of the tunnel were mapped more accurately than the last two SLAM algorithms. However, whenever there was a turn, some drifts in the sensor orientation occurred. The Cartographer node had an internal IMU tracker and scan matching functions to estimate poses and the internal functions were too sensitive to rotations. For instance, when the sensor rotated 90 degrees, the estimated rotation was about 180 degrees. Therefore, the algorithm couldn't achieve a loop closure and the map ended up having wrong rotation results.

# RTAB-Map
RTAB-Map (Real-Time Appearance-Based Mapping) provided an open-source Graph-Based SLAM library. It was designed to build 3D maps of environments using different sensors such as a RGB-D camera, stereo camera, and LiDAR sensor. For this experiment, the Realsense D435i camera data were used since RTAB-Map had been fully verified with the same type of cameras.

In this mapping algorithm, a keyframe-based approach was applied for mapping which meant a set of keyframes instead of a dense point cloud was used to represent the environment. This approach reduced the memory usage and computational requirements of the SLAM algorithm. This mapping algorithm also featured visual and loop closures to improve the accuracy of the map. Visual closures were detected by comparing the appearance of images captured by a camera, while loop closures were detected by comparing poses of the keyframes. Detailed instruction for running RTAB-Map in ROS can be found in [Appendix C](#appendix-c-rtab-map-in-ros).

<p style="text-align: center;"><iframe width="560" height="315" src="https://www.youtube.com/embed/l8Z8xgZt3Mc?si=2fxemMPZhNJ0YUig" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe><strong>Running RTAB-Map on Rviz.</strong></p>

<p style="text-align: center;"><img src="/assets/projects/RTAB-Map-1.png" width="500" height="500" /><img src="/assets/projects/RTAB-Map-2.png" width="300" height="300" /><strong><br />Map generated from RTAB-Map.</strong></p>

The point cloud map showed a very accurate map of the tunnel providing visual information such as a vending machine and elevator as well. There was a slight deviation in the top right corner of the map where the cart made a 180 degree turn. However, RTAB-Map instantly corrected the drift with visual closure. 

# ORB-SLAM 3

# Appendix A: Gmapping SLAM in ROS
This instruction assumes pre-installation and setup of ROS Noetic on Ubuntu 20.04.5.

### Installing Gmapping SLAM package
```bash
sudo apt-get update
sudo apt-get install ros-noetic-gmapping
cd catkin_ws
rosmake gmapping
```

### Installing `laser_scan_matcher` package
```bash
sudo apt-get install ros-noetic-scan-tools
```

### Running Gmapping SLAM
```bash
roscore
```
Open another terminal.
```bash
rosparam set use_sim_time true
rosrun rviz rviz
```
Open another terminal.
```bash
rosparam set use_sim_time true
rosrun gmapping slam_gmapping scan:=scan
```
Open another terminal.
```bash
rosparam set use_sim_time true
rosrun laser_scan_matcher laser_scan_matcher_node _fixed_frame:=odom _max_iterations:=10
```
Open another terminal.
```bash
rosparam set use_sim_time true
rosbag play --clock -d 5 tunnel.bag
```

<br />

# Appendix B: Cartographer SLAM in ROS

### Building Cartographer ROS
```bash
sudo apt-get update
sudo apt-get install -y python3-wstool python3-rosdep ninja-build stow
cd catkin_ws
wstool init src
wstool merge -t src https://raw.githubusercontent.com/cartographer-project/cartographer_ros/master/cartographer_ros.rosinstall
wstool update -t src
sudo rosdep init
```
Ignore the error meesages after `sudo rosdep init`.
```bash
rosdep update
rosdep install --from-paths src --ignore-src --rosdistro=${ROS_DISTRO} -y
```
<p style="text-align: center;"><img src="/assets/projects/Cartographer-Error.png" width="1000" height="250" /></p>
Resolve the error message above by removing line 46 (`<depend>libabsl-dev</depend>`) from the `package.xml` file in the cartographer (not cartographer ros) package.

```bash
src/cartographer/scripts/install_abseil.sh
sudo apt-get remove ros-${ROS_DISTRO}-abseil-cpp
catkin_make_isolated --install --use-ninja
```
Add the following in the `.bashrc` file in the home (`~`) directory.
```bash
source ~/catkin_ws/install_isolated/setup.bash
```
Reopen the terminal or source the above bash file.
```bash
cp install_isolated/share/cartographer_ros/configuration_files/backpack_2d.lua install_isolated/share/cartographer_ros/configuration_files/my_robot.lua
```
Use a code editor to open a Lua file (VSCode was used below).
```bash
code install_isolated/share/cartographer_ros/configuration_files/backpack_2d.lua
```
Modify the Lua file as follows:
<p style="text-align: center;"><img src="/assets/projects/Cartographer-Lua.png" width="400" height="150" /></p>
Finally, run Cartographer SLAM.

```bash
roslaunch cartographer_ros demo_my_robot.launch bag_filename:=/home/tyang/tunnel.bag
```

<br />

# Appendix C: RTAB-Map in ROS

### Rviz Setup
In order to visualize the point cloud map in Rviz, the following settings are required.
```bash
roscore
```
Open another terminal and follow the setup for `LaserScan`.
```bash
rosparam set use_sim_time true
rosrun tf static_transform_publisher 0 0 0 0 0 0 base_link map 100
```
Open another terminal and follow the setup for `PointCloud2`.
```bash
rosparam set use_sim_time true
rosrun tf static_transform_publisher 0 0 0 2 0 -1.57 base_link camera_depth_optical_frame 100
```

### Running RTAB-Map

```bash
roslaunch rtabmap_launch rtabmap.launch rtabmap_args:="--delete_db_on_start --Optimizer/GravitySigma 0.3" depth_topic:=/rgbd/camera/depth/image_rect_raw rgb_topic:=/rgbd/camera/color/image_raw camera_info_topic:=/rgbd/camera/color/camera_info approx_sync:=false wait_imu_to_init:=false \
```

Open another terminal.

```bash
rosparam set use_sim_time true
rosbag play --clock tunnel.bag
```




<br />
more accurate odometry data using IMU
more advanced sensor fusion algorithm using all sensor data
better comparison analysis with a ground truth