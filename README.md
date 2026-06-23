# jetson_ws

ROS2 Humble workspace for the Pepper robot with Unitree L2 LiDAR and Intel RealSense D435. Runs on Ubuntu 22.04 (Jetson / WSL2).

## Sensors

| Sensor | Driver | Topic |
|---|---|---|
| Unitree L2 LiDAR | `unilidar_sdk2` | `unilidar/cloud` |
| Intel RealSense D435 | `realsense-ros` | `/camera/color/image_raw` |

## Odometry packages

| Package | Method | Launch |
|---|---|---|
| `FAST_LIO_ROS2` | Tightly-coupled LiDAR-IMU | `ros2 launch fast_lio mapping.launch.py config_file:=l2.yaml` |
| `kiss-icp` | LiDAR-only, scan-to-map | `ros2 launch custom_launch kiss_icp_pepper.launch.py` |
| `point_lio_ros2` | Point-wise LiDAR-IMU | `ros2 launch point_lio_ros2 mapping_l2lidar_node.launch.py` |
| `l2lidar_node` | L2 preprocessing node | `ros2 launch l2lidar_node l2lidar.launch.py` |

## Clone the workspace

```bash
# Install vcstool if needed
pip install vcstool

# Clone and import all repos
git clone https://github.com/yohatad/jetson_ws.git
cd jetson_ws
vcs import src < workspace.repos
```

## Build

```bash
source /opt/ros/humble/setup.bash
rosdep install --from-paths src --ignore-src -r -y
colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release
source install/setup.bash
```

## Repository map

| Repo | Source | Notes |
|---|---|---|
| [FAST_LIO_ROS2](https://github.com/yohatad/FAST_LIO_ROS2) | fork of Ericsii | Pepper+L2 config, odom bridge |
| [kiss-icp](https://github.com/yohatad/kiss-icp) | fork of PRBonn | Pepper L2 config added |
| [l2lidar_node](https://github.com/yohatad/l2lidar_node) | fork of markgol | Pepper integration |
| [point_lio_ros2](https://github.com/yohatad/point_lio_ros2) | fork of dfloreaa | L2 config and launch |
| [realsense-ros](https://github.com/yohatad/realsense-ros) | fork of IntelRealSense | Pepper-specific patches |
| [unilidar_sdk2](https://github.com/yohatad/unilidar_sdk2) | fork of unitreerobotics | ROS2-only, launch updated |
| [custom_launch](https://github.com/yohatad/custom_launch) | in-house | Robot launch files |
| [direct_visual_lidar_calibration](https://github.com/koide3/direct_visual_lidar_calibration) | upstream | LiDAR-camera calibration |
| [ydlidar_ros2_driver](https://github.com/YDLIDAR/ydlidar_ros2_driver) | upstream | YDLIDAR driver |
