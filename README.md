# Robotics Lab Homework 3

## Table of Contents
1. [Overview](#overview)
2. [Constructing a Gazebo World and Detecting a Circular Object](#1-constructing-a-gazebo-world-and-detecting-a-circular-object)
   - [Creating a New World](#1a-creating-a-new-world)
   - [Creating a New Launch File](#1b-creating-a-new-launch-file)
   - [Detecting the Object](#1c-detecting-the-object)
3. [Modifying the Look-at-Point Vision-Based Control](#2-modifying-the-look-at-point-vision-based-control-example)
   - [Enhancing KDL Robot Vision Control](#2a-enhancing-kdl-robot-vision-control)
   - [Improved Look-at-Point Algorithm](#2b-improved-look-at-point-algorithm)
   - [Developing a Dynamic Vision-Based Controller](#2c-developing-a-dynamic-version-of-the-vision-based-controller)
4. [Results and Observations](#results-and-observations)
5. [How to Run the Simulation](#how-to-run-the-simulation)


## Overview
This project creates a **Gazebo simulation** for detecting a circular object using **OpenCV** and implementing a **vision-based control system** for a robotic manipulator using **ROS, Gazebo, and KDL**.

## 1. Constructing a Gazebo World and Detecting a Circular Object

### 1a) Creating a New World
- Created a **Gazebo world** within the `iiwa_gazebo` package.
- Added a **red-colored cylinder** (radius: **15 cm**) as a static object at `(x=1, y=-0.5, z=0.6)`.
- Used **SDF (Simulation Description Format)** to define its properties.
- Positioned it for clear camera visibility.

### 1b) Creating a New Launch File
- Developed `iiwa_gazebo_circular_object.launch` to **spawn the robot** with a camera.
- Verified visibility using `rqt_image_view`.

### 1c) Detecting the Object
- Used **opencv_ros** to process the camera feed.
- Applied **blob detection** to identify the circular object.
- Steps:
  - Convert image to grayscale.
  - Detect blobs.
  - Highlight detected object.
  ![Pac-Man Simulation](https://github.com/ValentinaGiannotti/Pac-Man-/blob/main/pac-man-simulation.gif)](https://github.com/ValentinaGiannotti/Homework3/blob/main/detection.png)


## 2. Modifying the Look-at-Point Vision-Based Control Example

### 2a) Enhancing KDL Robot Vision Control
- Modified **KDL robot vision control** to align the camera with an **Aruco marker**.
- Computed **position and orientation errors**.
- Tested performance by moving the marker.

### 2b) Improved Look-at-Point Algorithm
- Implemented a **null space approach** for robustness.
- Defined a **unit-norm axis tracking problem**.
- Applied the control law:
  ```math
  q̇ = k(LJ)†sd + Nq̇0
  ```
  - `LJ`: Task Jacobian matrix.
  - `sd`: Desired orientation vector.
  - `N`: Null-space projector.
- Compared joint velocities with and without null-space projection.

### 2c) Developing a Dynamic Vision-Based Controller
- Integrated **joint-space and Cartesian-space inverse dynamics control**.
- Used **cubic polynomial trajectories** for smooth motion.
- Employed **effort controllers** for torque-based control.
- Ensured **stable tracking** of both position and orientation.

## Results and Observations
- **Object Detection**: Successfully detected circular object.
- **Vision-Based Control**: Maintained alignment with Aruco marker.
   ![Pac-Man Simulation](https://github.com/ValentinaGiannotti/Homework3/blob/main/aruco.gif)
- **Dynamic Controller**: Ensured smooth, stable motion.
  

## How to Run the Simulation
```bash
# Step 1: Launch Gazebo Simulation
roslaunch iiwa_gazebo iiwa_gazebo_circular_object.launch

# Step 2: Start Object Detection
roslaunch opencv_ros camera.launch

# Step 3: Launch Vision-Based Controller
rosrun kdl_ros_control kdl_robot_vision_control

# Step 4: Run Effort-Based Controller
rosrun kdl_ros_control kdl_robot_vision_control_effort
```


