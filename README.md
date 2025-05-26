# Lucia
List of packages required to run Lucia
## Lucia Controller
### Motor & Encoder controller
[lucia_controller](https://github.com/iHaruruki/lucia_controller.git)
### Lucia URDF
[lucia_description](https://github.com/iHaruruki/lucia_description.git)
### urg_node2 (a ROS2 driver node for HOKUYO 2D LiDAR)
[urg_node2](https://github.com/Hokuyo-aut/urg_node2.git)
### orbbec astra_pro
[ros2_astra_camera](https://github.com/orbbec/ros2_astra_camera.git)
## SLAM
### slam_toolbox
[lucia_slam_toolbox](https://github.com/iHaruruki/lucia_slam_toolbox.git)
### cartographer
[lucia_cartographer](https://github.com/iHaruruki/lucia_cartographer.git)
## Navigation
[lucia_navigation2](https://github.com/iHaruruki/lucia_navigation2.git)
# Spina
## arm
[spina_arm_controll](https://github.com/iHaruruki/spina_arm_controll.git)
## vital
### vital measurement
[lucia_vital](https://github.com/iHaruruki/lucia_vital.git)
### vital calibration
[lucia_vital_calibration](https://github.com/iHaruruki/lucia_vital_calibration.git)
### vital display
[lucia_vital_signs_display](https://github.com/iHaruruki/lucia_vital_signs_display.git)

## command list
### lucia_navitaion2
```
$ ros2 run lucia_controller lucia_controller_node
$ ros2 launch urg_node2 urg_node2.launch.py
$ ros2 launch lucia_description robot.launch.py
$ ros2 launch lucia_navigation2 navigation2.launch.py map:=$HOME/ros2_ws/src/lucia_navigation2/map/map.yaml 
```

### spina_arm_controll
```shell
$ sudo chmod 777 /dev/ttyUSB0
$ ros2 run spina_arm_controll serial_controller_node
#### Set the overall angle to -90°
$ ros2 topic pub /angle_cmd std_msgs/msg/String "{ data: 'A0p-090' }" --once
#### If you want to control a single module, use "{ data: 'C1p+015' }".
$ ros2 topic pub /angle_cmd std_msgs/msg/String "{ data: 'C1p-030' }" --once
```
### vLucia_vital_signs_display
```
$ yarpmanager --application /home/robot/repos/robot/script/ymanager/xml/applications/tutorial/tutorial_audio_3.xml
$ ros2 run spina_arm_controll serial_controller_node
$ ros2 run lucia_vital vital_controller_node
$ ros2 run lucia_vital_signs_display vital_signs_display_node
```
### lucia_vital
```
$ ros2 run lucia_vital vital_controller_node
```
