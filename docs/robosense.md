# Robosense
## Requirements

### Power supply



Based on https://github.com/RoboSense-LiDAR/rslidar_sdk


## ROS integration

The default ROS package unfortunately has both ROS1 and ROS2 package files in the same directory with ROS1 as the default.
So there are some config changes required before we can build.

From the workspace root directory:

```
# Get the dependencies.
sudo apt-get update
sudo apt-get install -y libpcap-dev libyaml-cpp-dev
    
# Clone the repos.
git clone https://github.com/RoboSense-LiDAR/rslidar_sdk.git src/rslidar_sdk
git clone https://github.com/RoboSense-LiDAR/rslidar_msg.git src/rslidar_msg

# Move to repo dir.
cd src/rslidar_sdk
    
# Checkout known good version
git checkout tags/v1.5.13
    
# Get the submodules.
git submodule init
git submodule update
    
# Replace line in CMakeLists.txt
sed -i "s/set(COMPILE_METHOD CATKIN)/set(COMPILE_METHOD COLCON)/g" CMakeLists.txt

# Replace package.xml contents
cat package_ros2.xml > package.xml
    
# Move back to workspace.
cd ../..
    
# Build.
colcon build --symlink-install --packages-select rslidar_sdk rslidar_msg
```
