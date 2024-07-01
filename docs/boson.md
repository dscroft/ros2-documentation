# FLIR boson

## ROS

This is using David's fork and ROS2 rewrite available here [https://github.com/dscroft/flir_boson_usb](https://github.com/dscroft/flir_boson_usb).

At the moment the node is minimally functional but additional capabilities can be added if needed/requested.

### Installing 

1. From the workspace src directory.
2. Clone repo.
    - ```git@github.com:dscroft/flir_boson_usb.git```
3. Use known good version.
    - ```cd flir_boson_usb```
    - ```git checkout be3d5fc```

### Running

This has been written as a lifecycle node so it is nessicary to step through the configure and activate steps before the node will output anything, the demo launch file show how to do this automatically.

```ros2 launch flir_boson_usb boson_demo.py```

Or manually.

1. Run the node.
    - ```ros2 run flir_boson_usb_node``
2. Configure transition.
    - ```ros2 lifecycle set /flir_boson_usb configure```
3. Active transition.
    - ```ros2 lifecycle set /flir_boson_usb activate```