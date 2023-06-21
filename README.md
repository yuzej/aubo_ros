# aubo_ros
To solve the problem about aubo_i5 ros package.

## urdf
aubo_i5.urdf  (original)

aubo_i5_new.urdf.xacro 
(Solve problems in moveit_setup_assistant loading aubo_i5_robot.urdf.xacro, two issues)
- add the <xacro_macro> definitation for robot link and joints
- delete the include of material.xacro. (Issue 1: material 'black' is not unique.)
- delete the definitation of Link world and Joint world_joint. (Issue 2: link 'world' is not unique.)

aubo_it_robot_new.urdf.xacro
- include the newly aubo_i5 xacro file above, aubo_i5_new.urdf.xacro.
