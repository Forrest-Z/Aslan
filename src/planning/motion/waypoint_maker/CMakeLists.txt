cmake_minimum_required(VERSION 2.8.3)
project(waypoint_maker)

find_package(aslan_build_flags REQUIRED)

find_package(aslan_msgs REQUIRED)

#find_package(vector_map REQUIRED)
find_package(catkin REQUIRED COMPONENTS
        roscpp
        std_msgs
        geometry_msgs
        nav_msgs
        tf
        waypoint_follower
        aslan_msgs
        )

find_package(Boost REQUIRED)

catkin_package(
        CATKIN_DEPENDS
        roscpp
        std_msgs
        geometry_msgs
        nav_msgs
        tf
        waypoint_follower
        aslan_msgs
)

SET(CMAKE_CXX_FLAGS "-O2 -g -Wall ${CMAKE_CXX_FLAGS}")

include_directories(
        ${catkin_INCLUDE_DIRS}
        ${vector_map_INCLUDE_DIRS}
        ${aslan_msgs_INCLUDE_DIRS}
        ${Boost_INCLUDE_DIRS}
)

add_executable(waypoint_loader nodes/waypoint_loader/waypoint_loader_core.cpp nodes/waypoint_loader/velocity_replanner.cpp nodes/waypoint_loader/waypoint_loader_node.cpp)
target_link_libraries(waypoint_loader ${catkin_LIBRARIES})
add_dependencies(waypoint_loader
        ${catkin_EXPORTED_TARGETS})

add_executable(waypoint_saver nodes/waypoint_saver/waypoint_saver.cpp)
target_link_libraries(waypoint_saver ${catkin_LIBRARIES})
add_dependencies(waypoint_saver
        ${catkin_EXPORTED_TARGETS})

add_executable(waypoint_marker_publisher nodes/waypoint_marker_publisher/waypoint_marker_publisher.cpp)
target_link_libraries(waypoint_marker_publisher ${catkin_LIBRARIES})
add_dependencies(waypoint_marker_publisher
        ${catkin_EXPORTED_TARGETS})

install(TARGETS
        waypoint_loader
        waypoint_saver
        waypoint_marker_publisher
        ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
        )
