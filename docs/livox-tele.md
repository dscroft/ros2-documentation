# Livox Tele-15
## Requirements

### Power supply

12v.

### Install Livox SDK

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

Visualise using Livox Viewer.

1. Download from [https://www.livoxtech.com/downloads](https://www.livoxtech.com/downloads).
    - ```wget https://terra-1-g.djicdn.com/65c028cd298f4669a7f0e40e50ba1131/Download/update/Livox_Viewer_For_Linux_Ubuntu16.04_x64_0.10.0.tar.gz```
2. Extract.
    - ```tar xvf Livox_Viewer_For_Linux_Ubuntu16.04_x64_0.10.0.tar.gz```
3. Run.
    - ```cd Livox_Viewer_For_Linux_Ubuntu16.04_x64_0.10.0```
    - ```./livox_viewer.sh```
4. View.
    - Assuming lidar appears listed on the left hand side, click toggle button
    - Press play button on toolbar.

## ROS

This is based on a fork Livox ROS driver repo available here [https://github.com/acceleration-robotics/livox_ros2_driver](https://github.com/acceleration-robotics/livox_ros2_driver).
The fork fixes a number of bugs for building the driver under humble.

Livox has two different drivers for different lidar models.

### Installing
From the workspace root directory.
1. Clone repo.
    - `git clone https://github.com/acceleration-robotics/livox_ros2_driver.git src/livox_ros2_driver`
2. Use known good version.
    - `cd src/livox_ros2_driver`
    - `git checkout 88ab3b2`
3. Build.
   - `cd ../..`
   - `colcon build`

### Running
From the workspace root directory.
```bash
source install/setup.bash
ros2 launch livox_ros2_driver livox_lidar_rviz_launch.py
```
