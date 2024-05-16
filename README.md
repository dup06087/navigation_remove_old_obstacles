ROS navigation is not able to remove old costmap values. Even if there are observation_persistence and other parameters for removing old costmap values, It may not seem to work well.

I don't know it is suit for other ros version. This is for melodic. I downloaded ros-planning/navigation https://github.com/ros-planning/navigation at this site.

the branch is melodic-devel not noetic-devel and others.

Replace obstacle_layer.cpp obstacle_layer.h with your own.

obstacle_layer.cpp is on ~/catkin_ws/src/navigation/costmap_2d/plugins and obstacle_layer.h is on ~/catkin_ws/src/navigation/costmap_2d/include/costmap_2d

and

At obstacle_layer.cpp in line 78, last_update_times_.resize(200 * 200 , ros::Time::now()); // 각 셀의 마지막 업데이트 시간 초기화

change 200 * 200 to your own value, last_update_times_.resize(map_size_in_meters * resolution, map_size_in_meters * resolution, ros::Time::now());

map_size_in_meters, resolution are in yaml files.

and re- catkin_make

This makes grids to have their own update timestamp. And if time difference is bigger than "decay_time" in line 64, then cost removed to 0.
