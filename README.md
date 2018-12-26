
**This file describes steps to perform test on the robot localization package for ROS2 (crystal) and other required stuff.**

**Overview:**

	robot_localization is a package of nonlinear state estimation nodes.

**List of changes from ROS1 to ROS2:**

	The changes has been done by following the Migration guide for ROS2( https://index.ros.org/doc/ros2/Migration-Guide/ ).

	1. Migrated all test.cpp into ROS2 style
	2. Migrated yaml files into ROS2 style
	3. Migrated .test files into launch.py in ROS2 style

**Pre-requisites:**

	1. System should have installed crystal distro. Check out installation instructions and tutorials https://index.ros.org/doc/ros2/.
	2. System should have checkout ros2 robot_localization pkg & build (Refer Steps below).
	3. User should checkout diagnostics pkg & build (Refer steps below).
	4. System should have both ROS1 & ROS2. Note: We have verified on ROS1(i.e. melodic) & ROS2 (i.e. crystal).

		1. Steps to checkout diagnostics pkg & build:

			1. mkdir -p diagnostics_ws/src
			2. cd ~/diagnostics_ws/src
			3. git clone git@github.com:vaibhavbhadade/diagnostics.git //Note: branch should be remotes/origin/ros2-devel 
			4. cd ~/diagnostics_ws
			5. source /opt/ros/crystal/setup.bash
			6. colcon build
	   
		2. Steps to checkout robot_localization pkg & build:

			1. mkdir -p rl_ws_crystal/src
			2. cd ~/rl_ws_crystal/src
			3. git clone git@github.com:Rohita83/robot_localization.git
			#git branch -ra //This command will show different branches of main git on #kinetic
			4. cd robot_localization/
			5. git checkout -b ros2-crystal-devel remotes/origin/ros2-crystal-devel
			6. cd ~/rl_ws_crystal
			7. cd ~/diagnostics/install/setup.bash //This command will source to diagnostics pkg
			8. source /opt/ros/crystal/setup.bash
			9. colcon build

**Limitations:-** 

	1. colcon test does not work as launch.py files can not be executed/added with CMakeLists.txt as of now.
	2. Each test/launch.py files have been tested independently.
	3. Use of ros1 bridge to play rosbag files as rosbag is not available in ros2.
	4. Bag related testcases launch files will not terminate automatically i.e. user will have to do manually ctrl+C after bag duration finish.

**Future Work:-**

	1. All Test launch.py files should be executed using "colcon test" automatically. Currently we have built automated script "robot_localization_test_cases.sh" which performs all the test cases except bag related test cases.
	2. Remove the dependency with ros1_bridge to play bag file from ROS1 using rosbag.

**TESTING:**

There are two ways to run the test cases:

	1. Using automated scripts
	The script files are placed in the /test folder of robot localization.
	Go to the test folder path and run the shell scripts provided that you have downloaded the git source code explained above.

	2. Using launch files independently
	Below are the steps to run the test case independently using launch files.
	In order to run the UKF related test cases, follow same steps as of EKF. Just replace ekf with ukf.
	e.g. test_ukf_localization_node_bag1.launch.py

1)*******test_ekf_localization_node_bag1.launch.py*******

Terminal1:-
1. source /opt/ros/melodic/setup.bash
2. roscore

Terminal2:- (Run ros1_bridge):-
1. source ~/ros2_ws/install/setup.bash
2. source /opt/ros/melodic/setup.bash
3. ros2 run ros1_bridge dynamic_bridge --bridge-all-topics	

Terminal3:- (Play .bag from ROS1):-
1. source /opt/ros/melodic/setup.bash
2. rosparam set /use_sim_time true
3. rosbag play ~/rl_ws_crystal/src/robot_localization/test/test1.bag --clock -d 5

Terminal4:- (Launch TestCase launch.py):-
1. source /opt/ros/crystal/setup.bash
2. source ~/rl_ws_crystal/install/setup.bash
3. ros2 launch robot_localization test_ekf_localization_node_bag1.launch.py

Terminal5:- (Run static_transform_publisher):-
1. source /opt/ros/crystal/setup.bash
2. ros2 run tf2_ros static_transform_publisher 0 -0.3 0.52 -1.570796327 0 1.570796327 base_link imu_link


2)*******test_ekf_localization_node_bag2.launch.py*******

Terminal1:-
1. source /opt/ros/melodic/setup.bash
2. roscore

Terminal2:- (Run ros1_bridge):-
1. source ~/ros2_ws/install/setup.bash
2. source /opt/ros/melodic/setup.bash
3. ros2 run ros1_bridge dynamic_bridge --bridge-all-topics	

Terminal3:- (Play .bag from ROS1):-
1. source /opt/ros/melodic/setup.bash
2. rosparam set /use_sim_time true
3. rosbag play ~/rl_ws_crystal/src/robot_localization/test/test2.bag --clock -d 5

Terminal4:- (Launch TestCase launch.py):-
1. source /opt/ros/crystal/setup.bash
2. source ~/rl_ws_crystal/install/setup.bash
3. ros2 launch robot_localization test_ekf_localization_node_bag2.launch.py

3)*******test_ekf_localization_node_bag3.launch.py*******

Terminal1:-
1. source /opt/ros/melodic/setup.bash
2. roscore

Terminal2:- (Run ros1_bridge):-
1. source ~/ros2_ws/install/setup.bash
2. source /opt/ros/melodic/setup.bash
3. ros2 run ros1_bridge dynamic_bridge --bridge-all-topics	

Terminal3:- (Play .bag from ROS1):-
1. source /opt/ros/melodic/setup.bash
2. rosparam set /use_sim_time true
3. rosbag play ~/rl_ws_crystal/src/robot_localization/test/test3.bag --clock -d 5

Terminal4:- (Launch TestCase launch.py):-
1. source /opt/ros/crystal/setup.bash
2. source ~/rl_ws_crystal/install/setup.bash
3. ros2 launch robot_localization test_ekf_localization_node_bag3.launch.py

4)*******test_ekf_localization_node_interfaces.launch.py*******

Terminal1:- (Launch TestCase .launch.py)
1. source /opt/ros/crystal/setup.bash
2. source ~/rl_ws_crystal/install/setup.bash
3. ros2 launch robot_localization test_ekf_localization_node_interfaces.launch.py

5)*******test_ekf.launch.py*******

Terminal1:- (Launch TestCase .launch.py)
1. source /opt/ros/crystal/setup.bash
2. source ~/rl_ws_crystal/install/setup.bash
3. ros2 launch robot_localization test_ekf.launch.py

6)*******test_filter_base_diagnostics_timestamps.launch.py*******

Terminal1:- (Launch TestCase .launch.py)
1. source /opt/ros/crystal/setup.bash
2. source ~/rl_ws_crystal/install/setup.bash
3. ros2 launch robot_localization test_filter_base_diagnostics_timestamps.launch.py
