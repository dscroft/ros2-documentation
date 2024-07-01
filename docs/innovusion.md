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

1. Download the *ROS2_23.08.31-release-2.12-fk-rc4-rv1.zip* file.
2. Extract *innovusion-ros-humble-2.12.0-fk-rc4-rv1-x86-public.deb*.
3. Install.
    - ```sudo dpkg -i install innovusion-ros-humble-2.12.0-fk-rc4-rv1-x86-public.deb```
4. Run.
    - ```ros2 run innovusion publisher```

The ros node will not output anything until the pointcloud is enabled via the web interface, this can be done manually or can be triggered via the lidar's REST interface. Calling the REST interface can be triggered automatically via a launch file if needed.

```wget 172.168.1.10:8675/v1/pcs/enable?toggle=ON```

### Full version

This approach uses the innovusion fork of the nebula lidar package available at [https://github.com/Innovusion-Inc/nebula.git](https://github.com/Innovusion-Inc/nebula.git).

Don't follow the exact instructions on the repo README if you want to run the code alongside other nodes.

1. Open workspace in terminal.
2. ```git clone git@github.com:Innovusion-Inc/nebula.git src/nebula```
3. ```vcs import src << src/nebula/build_depends.repos```
4. ```rosdep install --from-paths src --ignore-src -y -r```
    - Add ```--os=ubuntu:jammy``` if using a non-ubuntu distro.
5. ```ros2 launch src/nebula/nebula_ros/launch/innovusion_launch_all_hw.xml```

