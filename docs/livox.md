# Livox HAP
## Requirements

### Power supply

12v.

### Install Livox SDK

This approach is best but only works if you have sudo rights to machine, the non-sudo approach is detailed below.

1. ```git clone https://github.com/Livox-SDK/Livox-SDK2.git /tmp/Livox-SDK2```
2. ```mkdir /tmp/Livox-SDK2/build```
3. ```cd /tmp/Livox-SDK2/build```
4. ```cmake .. && make -j```
5. ```sudo make install```

### Configure ethernet link.

Connect ethernet cable and set your network device to the following.

```
Ip: 192.168.1.5
Netmask: 255.255.255.0
Gateway: 192.168.1.1
```

Via command line (assuming link is eth0).

1. Set IP address and netmask.
    - ```sudo ip addr add 192.168.1.5/24 dev eth0```
2. Add default gateway.
    - ```sudo ip route add default via 192.168.1.1 dev eth0```
3. Bring link up.
    - ```sudo ip link set dev eth0 up```


## Test system

### Pointcloud visualisation

**TODO LivoxViewer2**

## ROS

This is based on the offical Livox ROS driver repo available here [https://github.com/Livox-SDK/livox_ros_driver2](https://github.com/Livox-SDK/livox_ros_driver2).
In theory this supports ROS1 and ROS2 but in reality there are some fixes required for ROS2.
Livox has two different drivers for different lidar models.

### Installing

1. From the workspace src directory.
2. Clone repo.
    - ```git clone git@github.com:Livox-SDK/livox_ros_driver2.git```
3. Use known good version.
    - ```cd livox_ros_driver2```
    - ```git checkout tags/1.2.4```
4. Fix bug in v1.2.4 CMakeLists.txt.
   - ```sed -i '288i set(LIVOX_INTERFACES_INCLUDE_DIRECTORIES "")' CMakeLists.txt```
5. Configure to use ROS2.
    - ```cp package_ROS2.xml package.xml```
6. Build with the following.
   - ```colcon build -DROS_EDITION=ROS2 -DHUMBLE_ROS=humble```
   - Or modify CMakeLists.txt to use correct version by default.
     - ```sed -i '1i\set(ROS_EDITION "ROS2")\nset(HUMBLE_ROS "humble")' src/livox_ros_driver2/CMakeLists.txt```

### Non-sudo approach

In the *livox_ros_driver2/CMakeLists.txt* file around line 250.

Replace: 

```
find_library(LIVOX_LIDAR_SDK_LIBRARY liblivox_lidar_sdk_shared.so /usr/local/lib REQUIRED)
```

With: 

```
#find_library(LIVOX_LIDAR_SDK_LIBRARY liblivox_lidar_sdk_shared.so /usr/local/lib REQUIRED)

Include(FetchContent)

# === get deps ===
FetchContent_Declare(
  Livox-SDK2
  GIT_REPOSITORY https://github.com/Livox-SDK/Livox-SDK2
  GIT_TAG         v1.2.5 # or a later release
)

# === compile sdk2 ===
FetchContent_MakeAvailable(Livox-SDK2)

# === hack the pre-existing defines to point at the _deps folder ===
set(LIVOX_LIDAR_SDK_LIBRARY livox_lidar_sdk_shared)
set(LIVOX_LIDAR_SDK_INCLUDE_DIR "${livox-sdk2_SOURCE_DIR}/include")
```

### Running

The contents of *livox_ros_driver2/config/HAP_config.json* need to match the ip addresses of the Lidar and host computer for the launch file to work correctly. 
If you are using the ip address specified above the the default config file should work for you.

- lidar_configs: ip: *match lidar ip*.
- HAP: host_net_info: cmd_data_ip: *match host ip*
- HAP: host_net_info: point_data_ip: *match host ip*
- HAP: host_net_info: imu_data_ip: *match host ip*

```ros2 launch livox_ros_driver2 rviz_HAP_launch.py```


