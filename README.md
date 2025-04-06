# ME5413_Final_Project_Group9

NUS ME5413 Autonomous Mobile Robotics Final Project

> Authors: Group9, [Liu Xiao](https://github.com/llliuxiao), [Ren Teng](https://github.com/1425T), [Liu Chen-an](https://github.com/songs-for-you), [Hao Yuzhi](https://github.com/carveshadow), [Li Shuo](https://github.com/YokeLiLee) and [Levi](https://github.com/RicardoCDUT)  

![Ubuntu 20.04](https://img.shields.io/badge/OS-Ubuntu_20.04-informational?style=flat&logo=ubuntu&logoColor=white&color=2bbc8a)
![ROS Noetic](https://img.shields.io/badge/Tools-ROS_Noetic-informational?style=flat&logo=ROS&logoColor=white&color=2bbc8a)
![C++](https://img.shields.io/badge/Code-C++-informational?style=flat&logo=c%2B%2B&logoColor=white&color=2bbc8a)
![Python](https://img.shields.io/badge/Code-Python-informational?style=flat&logo=Python&logoColor=white&color=2bbc8a)

![cover_image](src/me5413_world/media/gz_world.png)

## Dependencies

* System Requirements:
  * Ubuntu 20.04 (18.04 not yet tested)
  * ROS Noetic (Melodic not yet tested)
  * C++11 and above
  * CMake: 3.0.2 and above
* This repo depends on the following standard ROS pkgs:
  * `roscpp`
  * `rospy`
  * `rviz`
  * `std_msgs`
  * `nav_msgs`
  * `geometry_msgs`
  * `visualization_msgs`
  * `tf2`
  * `tf2_ros`
  * `tf2_geometry_msgs`
  * `pluginlib`
  * `map_server`
  * `gazebo_ros`
  * `jsk_rviz_plugins`
  * `jackal_gazebo`
  * `jackal_navigation`
  * `velodyne_simulator`
  * `teleop_twist_keyboard`
  * `teleop_twist_joystick`
* And this [gazebo_model](https://github.com/osrf/gazebo_models) repositiory

## Important Note Before Your Coding !!!

- **Unless you find significant bug in the main or master branch, do not push to them, please checkout you own branch, like dev-lx.**
- **Do not upload any large files(Not including your code). If necessary, please upload to your GoogleDrive or OneDrive and update README.**
- **Updata your Notion Page in time.**

## Installation

This repo is a ros workspace, containing three rospkgs:

* `interactive_tools` are customized tools to interact with gazebo and your robot
* `jackal_description` contains the modified jackal robot model descriptions
* `me5413_world` the main pkg containing the gazebo world, and the launch files

**Note:** If you are working on this project, it is encouraged to fork this repository and work on your own fork!

After forking this repo to your own github:

**Using [Clion](https://www.jetbrains.com/clion/) would be better**

```bash
# Clone your own fork of this repo (assuming home here `~/`)
git clone --recurse-submodules https://github.com/ME5413-Group9-2025/ME5413_Final_Project.git
cd ME5413_Final_Project

# Install python packages
pip install opencv-python
pip install torch==2.2.0 torchvision==0.17.0 torchaudio==2.2.0 --index-url https://download.pytorch.org/whl/cu121
pip install transformers==4.46.3

# Install all dependencies
rosdep update
rosdep install --from-paths src --ignore-src -r -y
sudo apt-get remove ros-noetic-abseil-cpp
sh src/cartographer/scripts/install_abseil.sh

# Build
catkin_make_isolated --use-ninja

# Build specified package
catkin_make_isolated --pkg <pkg_name> --use-ninja

# Source 
source devel_isolated/setup.bash
```

To properly load the gazebo world, you will need to have the necessary model files in the `~/.gazebo/models/` directory.

There are two sources of models needed:

* [Gazebo official models](https://github.com/osrf/gazebo_models)

  ```bash
  # Create the destination directory
  cd
  mkdir -p .gazebo/models
  
  # Clone the official gazebo models repo (assuming home here `~/`)
  git clone https://github.com/osrf/gazebo_models.git
  
  # Copy the models into the `~/.gazebo/models` directory
  cp -r ~/gazebo_models/* ~/.gazebo/models
  ```

* [Our customized models](https://github.com/NUS-Advanced-Robotics-Centre/ME5413_Final_Project/tree/main/src/me5413_world/models)

  ```bash
  # Copy the customized models into the `~/.gazebo/models` directory
  cp -r ~/ME5413_Final_Project/src/me5413_world/models/* ~/.gazebo/models
  ```

## Usage

### 0. Gazebo World

This command will launch the gazebo with the project world

```bash
# Launch Gazebo World together with our robot, you can change any sensor you want in this shell
roscd me5413_bringup/shell
sh start.sh
```

Here, manual teleoperation is automatically load, default input is Xbox joystick .

### 1. Bringup all

After launching **Step 0**, in the second terminal:

```bash
roslaunch me5413_bringup bringup.launch slam_method:=cartographer
```
This command will bringup the whole pipeline, including cartographer, navigation, schedule and perception.

## Student Tasks

### 1. Map the environment

* You may use any SLAM algorithm you like, any type:
  * 2D LiDAR
  * 3D LiDAR
  * Vision
  * Multi-sensor
* Verify your SLAM accuracy by comparing your odometry with the published `/gazebo/ground_truth/state` topic (`nav_msgs::Odometry`), which contains the gournd truth odometry of the robot.
* You may want to use tools like [EVO](https://github.com/MichaelGrupp/evo) to quantitatively evaluate the performance of your SLAM algorithm.

### 2. Using your own map, navigate your robot

* We have provided you a GUI in RVIZ that allows you to click and generate/clear the random objects in the gazebo world:

  ![rviz_panel_image](src/me5413_world/media/control_panel.png)

* From the starting point, move to one of the four given destination boxes at the end of the map:

  * Count the number of occurance of each type of box (e.g. box 1, 2, 3, 4, the box numbers are randomly generated)
  * Cross the bridge (the location of the bridge is randomly generated)
  * Unlock the blockade on the bridge by publishing a `true` message (`std_msgs/Bool`) to the `/cmd_open_bridge` topic
  * Dock at the destination box with the least number of occurance

## Contribution

You are welcome contributing to this repo by opening a pull-request

We are following:

* [Google C++ Style Guide](https://google.github.io/styleguide/cppguide.html),
* [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines#main),
* [ROS C++ Style Guide](http://wiki.ros.org/CppStyleGuide)

## License

The [ME5413_Final_Project](https://github.com/NUS-Advanced-Robotics-Centre/ME5413_Final_Project) is released under the [MIT License](https://github.com/NUS-Advanced-Robotics-Centre/ME5413_Final_Project/blob/main/LICENSE)
