# aubo_ros with FT sensor
To solve the problem about aubo_i5 ros package with robotiq FT300.

## Preconditions
Ubuntu 20, ROS noetic

aubo_robot and robotiq package are properl configured.

## urdf
aubo_i5.urdf  (original)

aubo_i5_new.urdf.xacro 
(Solve problems in moveit_setup_assistant loading aubo_i5_robot.urdf.xacro, two issues)
- add the <xacro_macro> definitation for robot link and joints (aubo_i5_new)
- delete the include of material.xacro. (Issue 1: material 'black' is not unique.)
- add the virtual joint and link of ee (tool0, wrist_3link_tool0_fixed_joint), similiar with UR. Sensor must mount on the virtual link, or have mistakes called "gazebo_ros_ft_sensor error, measurment_joint do not exists."

aubo_i5_robot_new_ft.urdf.xacro
- include the newly aubo_i5_new xacro file above, aubo_i5_new.urdf.xacro.
- include the FT_sensor macro (robotiq FT-300) and gazebo configuration
- add a cylinder peg after the sensor.
- add arm transmission xacro.

## aubo_i5_moveit_config
aubo_i5_new.srdf
- group chain, change tip link
- disable_collisions

moveit_planning_execution_(real)_ft.launch
- change to planning_context_ft.launch
- (for name with real), start the node of real ft_sensor name "rq_sensor"
- change to move_group_ft.launch

move_group_ft.launch
- change to planning_context_ft.launch

planning_context_ft.launch
- include newly urdf and srdf.

## Utilisation

1. rviz
   simulation, $roslaunch aubo_i5_moveit_config moveit_planning_execution_ft.launch robot_ip:=127.0.0.1
   real robot, $roslaunch aubo_i5_moveit_config moveit_planning_execution_real_ft.launch robot_ip:=192.168.1.40 (robot ip)
2. gazebo
   simulation, $roslaunch aubo_gazebo aubo_i5_gazebo_control.launch
3. ft data
   simulation, $rostopic echo /ft_sensor_topic
   real robot, first run $ roslaunch robotiq_ft_sensor rq_test_sensor ( set to zero)
              then $ rostopic echo /robotiq_ft_sensor or /robotiq_ft_wrench

## reference
1. aubo_robot
   https://www.bilibili.com/read/cv18052037
2. robotiq
   http://wiki.ros.org/robotiq
