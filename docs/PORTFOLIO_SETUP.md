# Setup and code scope

This is the final source snapshot of an unsuccessful driving project. Setup instructions describe the available software interfaces; they do not establish successful vehicle operation.

## Environment

- ROS 2 Humble and the ROS message/launch dependencies declared by the four packages.
- Python packages used by the selected nodes, including NumPy, OpenCV/cv_bridge, PyTorch, Ultralytics and PySerial as applicable.
- External YOLOPv2 source and weights for the perception path, plus input video or compatible camera topics.
- CARLA and its ROS bridge/messages for the earlier implementations under `src/neuro_decision/archive`.

Exact versions of external inference assets and CARLA were not recovered in this snapshot. The repository contains no trained weights, datasets, or test video. Package metadata alone is not a complete lockfile for the original environment.

## Build current packages

From the repository root in a suitable Linux ROS environment:

```bash
source /opt/ros/humble/setup.bash
colcon build --packages-select neuro_decision vehicle_serial_bridge yolopv2_ros autonomous_bringup
source install/setup.bash
```

`neuro_decision/setup.py` installs the current behavior, steering and optional `cmd_vel` adapter entry points. The earlier CARLA scripts under `archive/` are retained as source history and are not installed as those entry points.

## Mock interface inspection

To launch the current decision and bridge path without starting perception or opening a serial port:

```bash
ros2 launch autonomous_bringup e2e_mock.launch.py \
  mock_serial:=true enable_perception:=false
```

This composition still needs compatible perception and feedback topics to progress beyond missing-input/stop behavior. Inspect the topics in another sourced terminal:

```bash
ros2 topic echo /behavior_state
ros2 topic echo /desired_steering_angle_deg
ros2 topic echo /vehicle/mcu_tx
```

The [mock launch](../src/autonomous_bringup/launch/e2e_mock.launch.py) defaults to mock serial. The standalone [bridge YAML](../src/vehicle_serial_bridge/config/mcu_serial_bridge.yaml) instead specifies `mock_serial: false`; do not treat every launch/configuration as a mock run. The original [hardware preparation guide](hardware_preflight_checklist.md) and [bringup README](../src/autonomous_bringup/README.md) are preserved for the separate hardware path. No hardware connection was made during repository collection.

## Perception assets and paths

The perception launch defaults to paths from the original workstation:

- `/home/subin/YOLOPv2`
- `/home/subin/YOLOPv2/data/weights/yolopv2.pt`
- `/home/subin/test.mp4`

Replace these with matching local assets. The [perception launch](../src/yolopv2_ros/launch/perception_planning_bridge.launch.py) exposes `yolopv2_root`, `yolopv2_weights`, and `video_path`. The top-level mock launch forwards `video_path` but does not explicitly expose all model-path arguments. One inspection approach is to start the perception launch separately with its path overrides and keep `enable_perception:=false` in the mock composition.

## Interface details from the code

| Interface or setting | Current source |
| --- | --- |
| Target and state into steering | `/target_point` (`geometry_msgs/Point`), `/behavior_state` (`std_msgs/String`) |
| Steering output | `/desired_steering_angle_deg` (`std_msgs/Float64`), in degrees |
| Additional steering output | `/desired_steering_normalized` (`std_msgs/Float64`) |
| MCU speed input | `/desired_speed` (`std_msgs/Float64`) |
| Steering limit | Default 20 degrees |
| Steering update interval | Default 0.05 seconds |
| Steering input freshness | Default 0.5 seconds |
| Lane timeout | Decision launch: 0.5 seconds; mock bringup override: 2.0 seconds |

These are defaults and topic declarations, not measured vehicle performance. The original README contains older names and limits; inspect launch overrides and node code when reproducing a configuration.

## Preserved source limitations

- `localization.launch.py` refers to `config/ekf.yaml`, which is absent from this snapshot. That launch is also absent from the package's installed launch list.
- `perception/lane_detection/scripts/infer.py` uses `YOLO` without importing it. It passes syntax parsing but cannot run as written without addressing that import and supplying its assets.
- `perception/lane_detection/configs/dataset.yaml` contains shell heredoc wrapper lines, so it fails YAML parsing. A training run would need a valid dataset configuration and the corresponding data.
- Model/data paths and environment-dependent imports require the original or an equivalent external environment.

These findings concern standalone reproduction. They are not established causes of the project's unsuccessful driving outcome. The source is preserved unchanged; [validation](VALIDATION.md) records only the checks actually performed.
