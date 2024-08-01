# Robosense M1 Plus

Based on https://github.com/RoboSense-LiDAR/rslidar_sdk

## Requirements

### Network

Robosense defaults to 192.168.1.200 and communicates over 6699 and 7788.

Set host to:
  - Ip: 192.168.1.102.
  - Netmask: 255.255.255.0

Open ports:

```
sudo ufw allow from 192.168.1.200 to any port 6699
sudo ufw allow from 192.168.1.200 to any port 7788
```

### Power supply


## ROS integration

The default ROS package unfortunately has both ROS1 and ROS2 package files in the same directory with ROS1 as the default.
So there are some config changes required before we can build.

From the workspace root directory.
1. Get the dependencies.
    - `sudo apt update && sudo apt install -y libpcap-dev libyaml-cpp-dev`
2. Clone the repos (with submodules and at known working version).
   - `git clone -b v1.5.13 --depth 1 --recurse-submodules https://github.com/RoboSense-LiDAR/rslidar_sdk.git src/rslidar_sdk`
   - `git clone https://github.com/RoboSense-LiDAR/rslidar_msg.git src/rslidar_msg`
3. Configure to use ROS2.
    - `sed -i "s/set(COMPILE_METHOD CATKIN)/set(COMPILE_METHOD COLCON)/g" src/rslidar_sdk/CMakeLists.txt`
    - `cp src/rslidar_sdk/package_ros2.xml src/rslidar_sdk/package.xml`
4. Build.
   - `colcon build --symlink-install --packages-select rslidar_sdk rslidar_msg`


### Running
From the workspace root directory.
```bash
source install/setup.bash
ros2 launch rslidar_sdk start.py
```
