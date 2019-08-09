## Notes 

For investigating carla_autoware roslaunch sequence.

=============================================================================
### Setup environment paths
```
export CARLA_AUTOWARE_SOURCE=/media/z/Data/ws/carla-autoware
export CARLA_AUTOWARE_ROOT=/media/z/Data/ws/carla-autoware/autoware_launch
export CARLA_MAPS_PATH=/media/z/Data/ws/carla-autoware/autoware_data/maps
source $CARLA_AUTOWARE_SOURCE/catkin_ws/devel/setup.bash
export PYTHONPATH=$PYTHONPATH:~/carla-python/carla/dist/carla-0.9.6-py2.7-linux-x86_64.egg:~/carla-python/carla/
```

### Start Bridge
```
roslaunch $CARLA_AUTOWARE_ROOT/devel.launch
```
### Sort the structure of carla-autoware Bridge
```
  +--roslaunch $CARLA_AUTOWARE_ROOT/**devel.launch**
  |
  |
  |  # Start Controller                             
  |--$(find carla_autoware_bridge)/launch/            
  |--  **carla_autoware_bridge_with_manual_control.launch** 
  |
  |------# carla ROS manual control using WASD keys
  |------$(find carla_manual_control)/launch/ **carla_manual_control.launch**  # ROS-bridge pkg
  |
  |------# control carla using autoware
  |------$(find carla_autoware_bridge)/launch/**carla_autoware_bridge.launch**
  |
  |----------$(find carla_ros_bridge)/launch/**carla_ros_bridge.launch**  # ROS-bridge pkg
  |
  |----------$(find carla_ego_vehicle)/launch/carla_ego_vehicle.launch  # Setup Car with params
  |                ...
  |----------$(find carla_waypoint_publisher)/launch/***carla_waypoint_publisher.launch***
  |                ...
  |----------$(find carla_points_map_loader)/launch/carla_points_map_loader.launch  # Load point map
  |                ...
  |----------$(find carla_autoware_bridge)/launch/carla_autoware_bridge_common.launch
  |---------------*tf.launch*
  |---------------*topics relay for lidar&cam*
  |---------------``<node type="odometry_to_posestamped"/>`` 
  |---------------``<node type="carla_to_autoware_waypoints"/>``   # ***carla generates waypoint***                             \
  |  # Start necessary pkgs of Autoware
  |--$(env CARLA_AUTOWARE_ROOT)/autoware.launch
  |
  |------$(env CARLA_AUTOWARE_ROOT)/my_map.launch
  |          **...**
  |------$(env CARLA_AUTOWARE_ROOT)/my_sensing.launch
  |          **...**
  |------$(env CARLA_AUTOWARE_ROOT)/my_localization.launch
  |----------**ndt_matching.launch**
  |          **...**
  |------$(env CARLA_AUTOWARE_ROOT)/my_detection.launch
  |          **...**
  |------$(env CARLA_AUTOWARE_ROOT)/my_mission_planning.launch
  |----------*vel_pose_connect.launch*  # vel_pose_connect
  |----------``<node .. type="waypoint_marker_publisher"../>`` # Waypoint visualizer
  |          **...**
  |------$(env CARLA_AUTOWARE_ROOT)/my_motion_planning.launch
  |----------*costmap_generator.launch*
  |----------*astar_avoid.launch*   # **waypoint planner** 
  |----------*velocity_set.launch*  # **set local velocity**
  |----------*pure_pursuit.launch*
  |----------*twist_filter.launch*  # twist_filter 
```





