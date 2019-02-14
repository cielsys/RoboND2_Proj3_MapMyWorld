# RoboND2_Proj3_MapMyWorld 
ChrisL 2019-02-13

This repo is for personal coursework.<br/>
It is for the the "Map My World" ROS Slam Project, the third project of Udacity Robotic Engineer ND, Term2.

## Submission notes
NOTE: This repo contains more than just the Catkin source directory!
The ROS source directory is <br>
$REPO/catkin_ws/src

The project report is [report.pdf](./report/report.pdf)<br/>

## USAGE:

### Environment & Dependency Setup
This ROS project is only tested on Mint/Ubuntu16.04 with ROS Kinetic installed

It depends also on the RTABMap application. The instructions for building binaries from source are at

[Build RTABMap from source](https://github.com/introlab/rtabmap_ros#build-from-source)
 
**Summary for building RTABMap** in a cloud workspace:

Install build dependencies. 
```commandline
sudo apt-get install ros-kinetic-rtabmap ros-kinetic-rtabmap-ros
sudo apt-get remove ros-kinetic-rtabmap ros-kinetic-rtabmap-rossudo 
apt-get update
```

On my mint system I was able to get things working by just doing the install, 
not the remove and no build (I never got the build to succeed)
but there were many problems, workarounds and steps to make that work.

**Clone and build the RTABMap repo**
```commandline
cd /home/workspace 
git clone https://github.com/introlab/rtabmap.git rtabmap
cd rtabmap/build
cmake ..  [<---double dots included]
make
sudo make install
```

**LAST STEP MUST BE DONE EVERYTIME if using a fresh workspace!**
Also, make sure to clone this to /home/workspace, not ~/. so that the build
outputs are preserved for fresh workspace!

### Project ROS Setup

**Clone this repo and the RTABMap_ros repo**
```commandline
git clone https://github.com/cielsys/RoboND2_Proj3_MapMyWorld
cd RoboND2_Proj3_MapMyWorld/catkin_ws
git clone https://github.com/introlab/rtabmap_ros.git src/rtabmap_ros
```

I am not sure if these steps are actually needed. 
Or if they should be before or after catkin_make in next step
```commandline
rosdep install -i rtabmap_ros
rosdep install -i ciel_bot
```

**Build ROS packages**
```commandline
cd catkin_ws/
catkin_make
source devel/setup.bash
```

**Retrieve Gazebo models into ~/.gazebo**
```commandline
mkdir ~/.gazebo
curl -L https://s3-us-west-1.amazonaws.com/udacity-robotics/Term+2+Resources/P3+Resources/models.tar.gz | tar zx -C ~/.gazebo/
```
**MUST BE DONE EVERYTIME if using a fresh workspace!**
These models are also preserved in this repo in
$REPO/StudentProjectMaterials/models.tar.gz

Other supplied materials that came from
```commandline
wget https://s3-us-west-1.amazonaws.com/udacity-robotics/Term+2+Resources/P3+Resources/Student+Project+Materials.zip
```
are duplicated here but have already been installed and modified in the appropriate places in the 
$REPO/catkin_ws/src/ciel_bot package.

### Runtime Invocations
**Start ROS**
```commandline
cd catkin_ws/ 
source devel/setup.bash
roslaunch ciel_bot world_main.launch world_name:=kitchen_dining.world
```
use ```world_name:=custom.world``` for custom world.
This launches ROS, Gazebo and RViz with the robot and in the selected world.
RViz will launch with config from the repo.

In a 2nd shell **Start RTABMap_ros**
```commandline
cd catkin_ws/ 
source devel/setup.bash
roslaunch ciel_bot mapping.launch
```
This launches RTABMap_ros node and RTABView.
The RTABMap database is generated into 
catkin_ws/src/ciel_bot/rtabmap/rtabmap.db
This folder is git ignored because the files are so big. 

**The RTABMap process must be stopped with ctrl-c in shell#2**
in order to get the entire mapping database written out to file!

In a 3rd shell **Start teleop keyboard navigation**
```commandline
cd catkin_ws/ 
source devel/setup.bash
roslaunch ciel_bot teleop.launch
```
Keyboard input to this shell will drive the robot.

### Runtime Debugging and Utilities
``rosrun tf view_frames`` writes the ROS transform frames to ./frames.pdf.
``rosrun tf tf_monitor frame_1 frame_2`` monitors transforms.
``rqt_graph`` Displays the ros nodes and topics.
``roswtf`` Runs helpful diagnostics on the running ROS environment.
``rtabmap-databaseViewer ./rtabmap.db`` Loads a rtabmap database file for review.
``rostopic hz /rtabmap/odom`` Reports the update rate of a topic.
``rqt_console`` Reports log and error messages. 
``rqt_image_view`` A tool to view image topics, such as from the camera.

### Common problems
Get ROS to work initially in each shell. Can go in ~/.bashrc
``source /opt/ros/kinetic/setup.bash``

Fix the ROS IP address in each shell environment (eg IP)
``export ROS_IP=192.168.1.19``

Fix various broken things in the installed ROS environment.
POTENTIALLY DANGEROUS!
``apt-get update``

## Links
Submission Requirements: [Rubric](https://review.udacity.com/#!/rubrics/1441/view)
 