# ROS-Python3-env


## Introduction
This project is for using ROS-kinetic(Ubuntu16.04) or ROS-melodic in python3.6 environment, which contains several ROS source code packages.

---

To start using ROS command or a catkin_make/catkin_make_isolated built project, we have to set environment through 'source' command.
By sourcing /opt/ros/kinetic/setup.bash, we can use ROS commands such as 'rosrun' and 'rostopic'.
By sourcing ./devel/setup.bash or ./install/setup.bash in project folder, we can find customized packages and run with 'rosrun' command.
The main idea of source command is to add python lib paths before $PYTHONPATH. Therefore, the order of these lib paths determine which
lib to use when there are libs have the same name but different version.
The source code packages in this projects can be compiled with python3, and source the setup.bash of this workspace when using ROS in 
python3 environment.

This project contains packages below:
|
|----geometry
|----geometry2
|----ros_comm
|----vision_opencv

geometry contains tf module, geometry2 contains tf2 module, ros_comm contains ROS commands such as 'rosrun', vision_opencv contains ROS
message type conversion module 'cv_bridge'.
These packages are downloaded and picked from ROS offical source code. You can also download source code, but some CMakeLists.txt need to 
be modified.

## Requirements
* Ubuntu 16.04 with Anaconda (python3.6 environment) or Ubuntu 18.04 (system naturally conain python3.6)
* ROS-kinetic (ubuntu 16.04) or ROS-melodic (ubuntu 18.04)

## Installation
1. Source your python3.6 environment. Must be python3.6 as far as I know.
2. Download some modules in need:
```
pip install catkin_pkg rospkg empy pyyaml numpy
```
3. Download this project and typing commands below. The 'catkin' related command are still in python2.7 environment, thus source ROS
environment is still required, though using python3 specified arguments. You have to change python3.6 environment path in command
'catkin_make_isolated', the 'ros-py36' is a conda environment of mine. Using 'catkin_make_isolated' so that each package can be compiled
separately, for geometry and geometry2 have .cpp with the same name, if you use 'catkin_make' then error will occur.
(For ROS-melodic, you might have to modify vision_opencv/cv_bridge/CMakeLists.txt, change line 'Boost REQUIRED python-py35' to
'Boost REQUIRED python-py36')
```
source /opt/ros/kinetic/setup.bash
cd ROS-Python3-env
mkdir src
cd src
catkin_init_workspace
cd ..
catkin_make_isolated -DPYTHON_EXECUTABLE=/xxx/anaconda3/envs/ros-py36/bin/python -DPYTHON_INCLUDE_DIR=/xxx/anaconda3/envs/ros-py36/include/python3.6m -DPYTHON_LIBRARY=/xxx/anaconda3/envs/ros-py36/lib/libpython3.6m.so
```
4. Source this project and using ROS in python3.6.
```
source ./devel_isolated/setup.bash
```
5. I found that when using this environment, there's still conflict with python2.7 ROS environment, thus command below is recommened. You can also add
a 'alias' command in your ~/.bashrc to realize the same effect quickly. You have to change the python path to yours as well.
```
export PYTHONPATH=/home/anaconda3/envs/ros-py36/lib/python3.6/site-packages:$PYTHONPATH
```
The alias command are like:
```
alias <command_name>='export PYTHONPATH=/home/anaconda3/envs/ros-py36/lib/python3.6/site-packages:$PYTHONPATH'
```

### Source code
You can also download ROS source code through links below:
```
git clone https://github.com/ros-perception/vision_opencv.git src/vision_opencv
git clone https://github.com/ros/geometry
git clone https://github.com/ros/geometry2
git clone https://github.com/ros/ros_comm
```
Though some CMakeLists.txt are modified.
1. Change vision_opencv/cv_bridge/CMakeLists.txt, line 'Boost REQUIRED python37' to 'Boost REQUIRED python-py35'. And add
'set(CMAKE_CXX_STANDARD 11)' in the begining of .txt.
2. Change geometry2/tf2/CMakeLists.txt and geometry2/tf2_ros/CMakeLists.txt, add 'set(CMAKE_CXX_STANDARD 11)'.
3. For the ros_comm package, I refer to 'git clone https://github.com/Dysonsun/python3_ros_lib.git', thus some modification
are avoided.

Finally, wish you successfully set your ROS-python3 environment!

