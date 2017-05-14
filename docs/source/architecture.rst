Suricate robot tutorials
========================

This section explain some examples of how to use the gazebo simulation and ROS in order to play
with the robdos robot and develop some extensions.

this section assume you already have installed robdos project.

Play bag files
^^^^^^^^^^^^^^

In order to understand how variables changes inside the robot, let's play with a recorded bag files.

Remember download first the bag files in your computer and change the path into launch file (robdos_sim_play_bags.launch)

.. code-block:: none

    cd ~/catkin_ws
    source devel/setup.bash
    roslaunch robdos_sim robdos_sim_play_bags.launch


Once the launch file have started, a RVIZ window will open and show the movement of robot.


Waypoints using gazebo and RVIZ
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

You can execute a simple demo using waypoints (mavros_msgs/WaypointList), a simulated environment in gazebo and RVIZ.

.. code-block:: none

    cd ~/catkin_ws
    source devel/setup.bash
    roslaunch robdos_sim robdos_waypoints.launch

This command will open 2 main windows: a RVIZ viewer for waypoint visualization and a gazebo window with a simulated robot that execute the proper velocity commands.

The previous demo can be summarized in the following processes:

* Waypoint publisher

A python script (waypoint_publisher.py), it publishes a topic /mavros/mission/waypoints (type mavros_msgs/WaypointList) with a set of points to be reached by the robot.

it generates also a set of markers using topic /robdos/makers_waypoints (type visualization_msgs/Marker) for visualization using RVIZ.

* Waypoint controller

A python script (waypoint_controller.py), it publishes a velocity command for mavlink using topic /mavros/rc/out (type mavros_msgs/RCOut).

It subscribes for odometry information (position/orientation) to /robdos/odom (type nav_msgs/Odometry) (from gazebo simulation but also possible to subscribe for real odometry topic)

It also subscribes for waypoint list /mavros/mission/waypoints in order to perform a simple controller.

This demo only considers the first waypoint, but it will include the other coming soon.

* Gazebo plugin (gazebo_sub_drive)

In order to perform a realistic simulation of the robot, we have develop gazebo plugin that publishes simulated odometry and subscribes for desired velocity commands (mavros_msgs/RCOut).

This node convert RCOut values into valid angular velocity for thrusters, then, the Lift-Drag plugin and gazebo computes the resulting force over the robot.









