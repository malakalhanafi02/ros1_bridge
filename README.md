# ros1_bridge
The ros1_bridge package allows for communication between ROS1 and ROS2 systems. This task involves setting up the bridge to relay topics from ROS1 Noetic to ROS2 Foxy.

1. Install colcon:

   - Update your package index:
```bash
sudo apt update
```
   - Install colcon:
```bash
sudo apt install python3-colcon-common-extensions
```

2. Create Workspaces for ROS Noetic and ROS2 Foxy:

ROS1:
```bash
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin_make
source devel/setup.bash
```
ROS2:
```bash
mkdir -p ~/ros2_ws/src
cd ~/ros2_ws/
colcon build
source install/local_setup.bash
```
3. Install the Arduino Robot Arm Package in ROS Noetic:
   
   - Install moveit_ros_planning:
```bash
sudo apt-get install ros-noetic-moveit
```
   - Rebuild the Workspace (if encountered an error):
```bash
cd ~/catkin_ws
catkin_make
```
   - Install the Arduino Robot Arm Package:
```bash
cd ~/catkin_ws/src
git clone https://github.com/smart-methods/arduino_robot_arm.git
cd ..
catkin_make
source devel/setup.bash
```
4. Create and Set Up ros1_bridge Workspace:
```bash
mkdir -p ~/ros1_bridge_ws/src
cd ~/ros1_bridge_ws/src
git clone -b foxy https://github.com/ros2/ros1_bridge.git
cd ~/ros1_bridge_ws
colcon build --packages-select ros1_bridge --cmake-force-configure --cmake-args -DBUILD_TESTING=FALSE
source install/local_setup.bash
```
5. Source ROS1 and ROS2 Setup Files:
   
   - Ensure All Required ROS 2 Packages are Installed:
```bash
sudo apt update
sudo apt install ros-foxy-rmw-cyclonedds-cpp ros-foxy-rmw-fastrtps-cpp ros-foxy-rmw-implementation
```
   - Source ROS1 and ROS2
```bash
source /opt/ros/noetic/setup.bash
source ~/catkin_ws/devel/setup.bash
source /opt/ros/foxy/setup.bash
source ~/ros2_ws/install/local_setup.bash
```
  - If error encountered:
   - Install python3-colcon-common-extensions and python3-ament-package:
```bash
sudo apt update
sudo apt install python3-colcon-common-extensions python3-ament-package
```
   - Rebuild the ros1_bridge:
```bash
cd ~/ros1_bridge_ws
colcon build --packages-select ros1_bridge --cmake-force-configure --cmake-args -DBUILD_TESTING=FALSE
```
6. Test the bridge:
```bash
source install/local_setup.bash
ros2 run ros1_bridge dynamic_bridge --print-pairs
```
7. Launch the Arduino_robot_arm package:
```bash
source /opt/ros/noetic/setup.bash
source ~/catkin_ws/devel/setup.bash
roslaunch robot_arm_pkg check_motors.launch
rostopic list
```

<img width="1024" alt="image" src="https://github.com/malakalhanafi02/ros1_bridge/assets/122760944/c277e496-63a3-4fe4-a8d8-7bea3d2ecd24">

ERRORS: 

# Running ros1_bridge
After launching the _check_motors.launch_ file and have the robot arm running in ROS1, you need to set up the _ros1_bridge_ and run it to ensure communication between ROS1 and ROS2.

Open a new terminal:

8. Run the bridge: 
```bash
source install/setup.bash
ros2 run ros1_bridge dynamic_bridge
```
9. Echo /joint_states topic in ROS2:
```bash
source /opt/ros/foxy/setup.bash
ros2 topic echo /joint_states sensor_msgs/msg/JointState
```

