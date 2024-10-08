# Innovusion

## Requirements

### Configure ethernet link.

Connect ethernet cable and set your network device to the following.

```
Ip: 172.168.1.15
Netmask: 255.255.255.0
Gateway: 172.168.1.1
```

Via command line (assuming link is eth0).

1. Set IP address and netmask.
    - ```sudo ip addr add 172.168.1.15/24 dev eth0```
2. Add default gateway.ne
    - ```sudo ip route add default via 172.168.1.1 dev eth0```
3. Bring link up.
    - ```sudo ip link set dev eth0 up```


## Test system

### Pointcloud visualisation

1. Open browser.
2. Vist 172.168.1.10:8675
3. Open "Sensor config"
4. Run "Internal point cloud service"


## ROS integration

### Simple version

This version relies on installation of a package on your system.
It is possible that there are configuration option available for this package but we lack any documentation so it's unclear what they are or how to access them.

1. Download the *ROS2_23.08.31-release-2.12-fk-rc4-rv1.zip* file from the *resources* folder.
2. Extract *innovusion-ros-humble-2.12.0-fk-rc4-rv1-x86-public.deb*.
    - ```unzip ROS2_23.08.31-release-2.12-fk-rc4-rv1.zip```
3. Install.
    - ```sudo dpkg -i install 23.08.31-release-2.12-fk-rc4-rv1/innovusion-ros-humble-2.12.0-fk-rc4-rv1-x86-public.deb```
4. Run.
    - ```ros2 run innovusion publisher```

The ros node will not output anything until the pointcloud is enabled via the web interface, this can be done manually or can be triggered via the lidar's REST interface. Calling the REST interface can be triggered automatically via a launch file if needed.

```curl 172.168.1.10:8675/v1/pcs/enable?toggle=ON```

### Full version

This approach uses the innovusion fork of the nebula lidar package available at [https://github.com/Innovusion-Inc/nebula.git](https://github.com/Innovusion-Inc/nebula.git).

Don't follow the exact instructions on the repo README if you want to run the code alongside other nodes.

1. From the workspace **root** directory.
2. ```git clone https://github.com/Innovusion-Inc/nebula.git src/nebula```
3. ```vcs import src << src/nebula/build_depends.repos```
4. ```rosdep install --from-paths src --ignore-src -y -r```
    - Add ```--os=ubuntu:jammy``` if using a non-ubuntu distro.
5. Specify the sensor model as Falcon:
    - ```sed -i '2s/<arg name="sensor_model" description="Robin\/Falcon"/<arg name="sensor_model" default="Falcon" description="Robin\/Falcon"/' src/nebula/nebula_ros/launch/innovusion_launch_all_hw.xml```
5. `colcon build`

### Running
From the workspace root directory.
```bash
ros2 launch src/nebula/nebula_ros/launch/innovusion_launch_all_hw.xml
```
#### To use it with rviz2
When using rviz, the point cloud is rotated incorrectly, use a static transform publisher in a separate terminal (CTRL+ALT+T) to rotate it 90 degrees on the Y axis:
```bash
ros2 run tf2_ros static_transform_publisher 0 0 0 0 1.5708 0 innovusion innovusion_rotated
```
- In a separate terminal, open rviz with `rviz2`
- In rviz, under *Global Options*, change *Fixed Frame* to *innovusion_rotated*, if you're not using the static transform publisher, the *Fixed Frame* will be *innovusion*
- In the bottom left click *Add*, change the list to *By topic*, select *PointCloud2* under *innovusion_points*
