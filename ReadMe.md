# Gazebo World exaple

## step one: no ROS

- robot with lidar and commanded with arrows
- no ROS use
To run my warehouse world:

```bash
export GZ_SIM_RESOURCE_PATH=/home/gborghesan/gazebo_projects/local_models
gz sim   gazebo_example/warehouse_world.sdf

```

To visulise lidar, use the plugin (plugins can by opened by 3 dots on the top right corner of the world) __Visualise Lidar__.
It will work only if the simulation is running.

## step two with ROS

### lauch with ros2 
info at:
https://gazebosim.org/docs/harmonic/ros2_launch_gazebo/

basic launch command:

```bash
export GZ_SIM_RESOURCE_PATH=/home/gborghesan/gazebo_projects/local_models
ros2 launch ros_gz_sim gz_sim.launch.py gz_args:=blue_in_small_warehouse.sdf
```
(export is need only once for shell)

### TODO make a roslaunch file

### Interact with topics from gazebo

_Reminder:_ to visualise the ropics of gazebo:
```bash
gz topic -l
```
this allows to make a bridge, 

```bash
ros2 run ros_gz_bridge parameter_bridge /lidar@sensor_msgs/msg/LaserScan@gz.msgs.LaserScan
```

But it is very annoyng, so there is a yaml file for this.
https://github.com/gazebosim/ros_gz/blob/ros2/ros_gz_bridge/README.md#example-5-configuring-the-bridge-via-yaml

example:
https://github.com/gazebosim/ros_gz_project_template/blob/main/ros_gz_example_bringup/config/ros_gz_example_bridge.yaml


```yaml
- ros_topic_name: "lidar"
  gz_topic_name: "/lidar"
  ros_type_name: "sensor_msgs/msg/LaserScan"
  gz_type_name: "gz.msgs.LaserScan"
- ros_topic_name: "/tf"
  gz_topic_name: "/model/blue_robot/tf"
  ros_type_name: "tf2_msgs/msg/TFMessage"
  gz_type_name: "gz.msgs.Pose_V"
  direction: GZ_TO_ROS
```
to run
```bash
ros2 run ros_gz_bridge parameter_bridge --ros-args -p config_file:=/home/gborghesan/gazebo_projects/gazebo_example/bridge.yaml
```

to visualise the lidar data in rviz2 THe re is alos the need of making a TF trasformation between the odometry frame and the lidar frame

```bash
ros2 run tf2_ros static_transform_publisher "0" "0" "0" "0" "0" "0" "blue_robot/chassis" "blue_robot/chassis/gpu_lidar"
```
this is just a s short cut for the `robot publsher`. 

Prabably fixed in the next section.

## TODO check out it works with the package teplate

https://github.com/gazebosim/ros_gz_project_template/tree/main