<div align="center">

# 🤖 Lucia & Spina System  
*Autonomous mobile healthcare robot*  

[![ROS 2 Distro - Humble](https://img.shields.io/badge/ROS2-Humble-1f425f.svg)](https://docs.ros.org/)
[![ROS 2 Distro - Jazzy](https://img.shields.io/badge/ROS2-jazzy-1f425f.svg)](https://docs.ros.org/)
</div>

## 🧩 Component Overview

| Device | Description |
|----------|----------------|
| Lucia (Base) | motors, LiDAR, Depth-Camera, SLAM, Navigation |
| Spina (Arm) | serial control & inverse kinematics |
| Vital | Vital sign measurement |

---

## 📦 Package Matrix

| Domain | Device Name | Function | Repository |
|--------|----------|---------------|------------|
| Base | | Motor & Encoder Controller | [lucia_controller](https://github.com/iHaruruki/lucia_controller) |
| Base | | URDF / Description | [lucia_description](https://github.com/iHaruruki/lucia_description) |
| Sensing | UTM-30LX | LiDAR Driver | [urg_node2_setup](https://github.com/iHaruruki/urg_node2_setup.git) |
| Sensing | |Dual LiDAR Merger | [dual_laser_merger](https://github.com/iHaruruki/dual_laser_merger) |
| Sensing | Astra 2 | Depth Camera | [ros2_astra_camera_setup](https://github.com/iHaruruki/OrbbecSDK_ROS2_setup.git) |
| Sensing | Astra Stereo S U3 | Depth Camera | [OrbbecSDK_ROS2_setup](https://github.com/iHaruruki/OrbbecSDK_ROS2_setup.git) |
| Sensing | FLIR C2 | Thermal camera | [flir_c2_ros2](https://github.com/iHaruruki/flir_c2_ros2.git) |
| Sensing | Tatto | Touch Sensor | [tatto_pkg](https://github.com/iHaruruki/tatto_pkg.git) |
| Mapping | | slam_toolbox | [lucia_slam_toolbox](https://github.com/iHaruruki/lucia_slam_toolbox) |
| Mapping | | Cartographer | [lucia_cartographer](https://github.com/iHaruruki/lucia_cartographer) |
| Mapping | | Visual SLAM | [lucia_rtabmap_ros2](https://github.com/iHaruruki/lucia_rtabmap_ros2.git) |
| Navigation | |  Navigation2 | [lucia_navigation2](https://github.com/iHaruruki/lucia_navigation2) |
| Maps | | Map Storage | [maps](https://github.com/iHaruruki/maps) |
| Arm | | Arm Serial Control | [spina_arm_controll](https://github.com/iHaruruki/spina_arm_controll) |
| Arm | | Inverse Kinematics | [spina_inverse_kinematics](https://github.com/iHaruruki/spina_inverse_kinematics) |
| Vital | | Vital Measurement | [lucia_vital](https://github.com/iHaruruki/lucia_vital) |
| Vital | | Vital Measurement Feedback | [lucia_vital_signs_display](https://github.com/iHaruruki/lucia_vital_signs_display) |
| Audio | | capture, stream, and play back audio | [audio_common](https://github.com/iHaruruki/audio_common.git)
| Audio | | Text to Speech(JP) | [voicevox_tts_ros2](https://github.com/iHaruruki/voicevox_tts_ros2.git) |
| Audio | | Text to Speech(EN) | [audio_generator_edge_tts](https://github.com/iHaruruki/audio_generator_edge_tts.git) |
| Display | | Display robot status | [makuhari_gui](https://github.com/iHaruruki/makuhari_gui.git) |
---

# 事前準備

## Lucia電源
Luciaはバッテリーモードと外部電源モードの2種類の電源供給方法がある  

### バッテリーモード

Luciaの主電源を入れる 

### 外部電源モード

Luciaの主電源を入れる 

## NUC39 & NUC40への電源供給


## ネットワーク設定
1. NUC39 & NUC41にLANケーブルを接続する
2. NUC25 & NUC39 & NUC41をSSID(`deco_d2`)に接続する（Wi-Fi接続）

## YARP起動
1. NUC21を起動する
2. YARPモジュールの`Lucia-04-Green-01-Main`を起動  
3. 非常停止ボタンを解除する
4. モードを[Remote] モードに変更

---

# Lucia have 3 mode

## :video_game: A. Manual Control mode

### Startup control system and LiDAR 
```bash
# NUC39
ros2 launch lucia_controller bringup.launch.py 
```

### Control via keyboard
```bash
# NUC39
ros2 launch lucia_controller keyboard_teleop.launch.py
```

### Control via Joystick
```bash
# NUC39
ros2 launch lucia_controller joystick_teleop.launch.py
```

### Control via Remote Joystick
```bash
# NUC 25
ros2 launch lucia_controller makuhari_joystick_teleop.launch.py
```

> [!TIP]
> More information about the **lucia_controller** can be found in the following GitHub repository:  
> [lucia_controller](https://github.com/iHaruruki/lucia_controller)
---

## 🗺️ B. SLAM (Map Building)
  
### Startup control system and LiDAR

```bash
# NUC39
ros2 launch lucia_controller bringup.launch.py
```

### Startip Depth camera & Web camera
Astra2
```bash
# NUC41
ros2 launch lucia_controller astra2.launch.py
```
Brio 100
```bash
# NUC39
ros2 launch lucia_controller brio_100.launch.py
```

### Run slam_toolbox

```bash
# NUC39
ros2 launch lucia_slam_toolbox online_async.launch.py
```

### Control via Joystick
```bash
# NUC39
ros2 launch lucia_controller joystick_teleop.launch.py
```
### rosbag / カメラ画像を録画する
<!-- ```bash
#NUC39
cd ~/ros2_ws/rosbag
ros2 bag record --topics /camera/color/camera_info /camera/color/image_raw/compressed /camera/depth/camera_info /camera/depth/image_raw/compressedDepth /tf /tf_static /joint_states ⁠/camera/accel/imu_info ⁠/camera/accel/sample ⁠/camera/gyro/info ⁠/camera/gyro/sample ⁠/camera/gyro_accel/sample
``` -->
```bash
#NUC39
cd ~/ros2_ws/rosbag
ros2 bag record -a --exclude-topics /camera/color/image_raw /camera/color/image_raw/theora /camera/depth/image_raw /camera/depth/image_raw/theora /camera/depth/image_unaligned /image_unaligned/compressedDepth /camera/depth/image_unaligned/theora /camera/depth/image_unaligned/zstd /camera/depth/points /camera/ir/image_raw
```

NUC39とモニタを切り離す（HDMIを外す）

```bash
#NUC25
rviz2 -d ~/ros2_ws/src/lucia_slam_toolbox/rviz/lucia_slam_toolbox.rviz
```

*Start exploring and drawing the map.*
![slam_toolbox](/media/slam_toolbox.gif)

### Save the map you created

```bash
# NUC39
cd ~/ros2_ws/maps
ros2 run nav2_map_server map_saver_cli -f ~/ros2_ws/maps/map_01
# If the `maps` directory does not exist　
# mkdir -p ~/ros2_ws/maps
```
保存した地図を確認する
```bash
# NUC39
eog ~/ros2_ws/maps/map_01/map_01.pgm
```

> [!NOTE]
> The -f option specifies a folder location and a file name where files to be saved.  
> With the above command, map.pgm and map.yaml will be saved in the home folder `~/(/home/${username})`.

> [!TIP]
> More information about the **lucia_slam_toolbox** can be found in the following GitHub repository:  
> [lucia_slam_toolbox](https://github.com/iHaruruki/lucia_slam_toolbox.git)

---

## 🧭 C. Navigation (Using Saved Map)
  
### Startup control system and LiDAR
```bash
# NUC39
ros2 launch lucia_controller bringup.launch.py
```

### Startip Depth camera & Web camera
Astra2
```bash
# NUC41
ros2 launch lucia_controller astra2.launch.py
```
Brio 100
```bash
# NUC39
ros2 launch lucia_controller brio_100.launch.py
```
### rosbag / カメラ画像を録画する
<!-- ```bash
#NUC39
cd ~/ros2_ws/rosbag
ros2 bag record --topics /camera/color/camera_info /camera/color/image_raw/compressed /camera/depth/camera_info /camera/depth/image_raw/compressedDepth /tf /tf_static /joint_states ⁠/camera/accel/imu_info ⁠/camera/accel/sample ⁠/camera/gyro/info ⁠/camera/gyro/sample ⁠/camera/gyro_accel/sample
``` -->
```bash
#NUC39
cd ~/ros2_ws/rosbag
ros2 bag record -a --exclude-topics /camera/color/image_raw /camera/color/image_raw/theora /camera/depth/image_raw /camera/depth/image_raw/theora /camera/depth/image_unaligned /image_unaligned/compressedDepth /camera/depth/image_unaligned/theora /camera/depth/image_unaligned/zstd /camera/depth/points /camera/ir/image_raw
```
### Run navigation2
```bash
# NUC39
ros2 launch lucia_navigation2 bringup.launch.py \
  map:=$HOME/ros2_ws/maps/map_01
```

NUC39とモニタを切り離す（HDMIを外す）

```bash
#NUC25
rviz2 -d ~/ros2_ws/src/lucia_navigation2/rviz/lucia_rviz.rviz
```

### Initial Pose / ロボットの初期位置を設定する

  1. Click the `2D Pose Estimate` button in the RViz2 menu. / rviz2の`2D Pose Estimate`をクリックする  
  2. Click on the map where the actual robot is located and drag the large green arrow toward the direction where the robot is facing. / ロボットが配置されている地図上の位置をクリックし，大きな緑色の矢印をロボットが向いている方向へドラッグしてください  
  3. Repeat step 1 and 2 until the LiDAR sensor data is overlayed on the saved map. / LiDARセンサーデータが保存済みマップ上に重ねられるまで，手順1と2を繰り返す
  ![nav2_initial](/media/nav2_initial.gif)

### Send Navigation Goal / ロボットの目標地点を設定する

  1. Click the `2D Goal Pose` button in the RViz2 menu. / rviz2の`2D Goal Pose`をクリックする  
  2. Click on the map to set the destination of the robot and drag the green arrow toward the direction where the robot will be facing. / 地図をクリックしてロボットの目的地を設定し，緑の矢印をロボットが向く方向へドラッグしてください．
  ![nav2_goal](/media/nav2_goal.gif)

> [!TIP]
> More information about the **lucia_navigation2** can be found in the following GitHub repository:  
> [lucia_navigation2](https://github.com/iHaruruki/lucia_navigation2.git)  
> 
> **lucia waypoint follow mode**
> ![lucia_waypoint_follow_mode](/media/lucia_waypoint_follow.gif)
---

<!-- ## Spina Arm Control & Vital Signs Display System

### 🦾 Spina Arm Control

```bash
sudo chmod 777 /dev/ttyUSB0  # or add to dialout group
```
```bash
ros2 run spina_arm_controll serial_controller_node
```
```bash
# 例: 全体角度 +90° / Example command
ros2 topic pub /angle_cmd std_msgs/msg/String "{ data: 'A0p-090' }" --once
```
---

### 💓🔊 Vital Signs Display System

```bash
ros2 run spina_arm_controll serial_controller_node
```
```bash
ros2 run lucia_vital vital_controller_node
```
```bash
ros2 run lucia_vital_signs_display vital_audio_guidance_node
```
---

## 🧪 Debug
### Announce that you have arrived at the goal point
Navigation / Simulate Nav Success
```bash
ros2 topic pub \
  /navigate_to_pose/_action/status \
  action_msgs/msg/GoalStatusArray \
  "{status_list:
    [
      {
        goal_info:
          { stamp: {sec: 0, nanosec: 0},
            goal_id: {uuid: [0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1]} },
        status: 4
      }
    ]
  }" --once
```
`status: 4` = SUCCEEDED -->

---

## 🛠 Troubleshooting
:watch: Coming soon
