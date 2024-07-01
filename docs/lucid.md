# Lucid

## Requirements

As per https://support.thinklucid.com/using-ros2-for-linux/

### Arena SDK

1. Download Arena SDK.
    - Visit [https://thinklucid.com/downloads-hub/](https://thinklucid.com/downloads-hub/)
    - Get *ArenaSDK_v0.1.90_Linux_x64.tar.gz*
2. Ensure system up to date.
    - ```sudo apt-get update 2>&1 >/dev/null```
3. Extract.
    - ```areanasdk="ArenaSDK_v0.1.90_Linux_x64.tar.gz"```
    - ```installdir="$HOME/ArenaSDK"```
    - ```mkdir -p $installdir```
    - ```tar -xf $areanasdk -C $installdir```
4. Configure.
    - ```cd $installdir/ArenaSDK_Linux_x64```
    - ```sudo sh Arena_SDK_Linux_x64.conf```
5. Add to paths.
    - ```echo "export ARENA_ROOT=$installdir/ArenaSDK_Linux_x64" >> ~/.bashrc```
    - ```source ~/.bashrc```

### ROS driver

https://github.com/autowarefoundation/lucid_vision_driver
