# Catkin User Guide: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/user_guide.html
# Catkin CMake Standard: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/standards.html
cmake_minimum_required(VERSION 2.8.3)
project(distance_field)
# Load catkin and all dependencies required for this package
# TODO: remove all from COMPONENTS that are not catkin packages.
find_package(catkin REQUIRED COMPONENTS visualization_msgs moveit_msgs geometric_shapes rosconsole eigen)

# catkin_package parameters: http://ros.org/doc/groovy/api/catkin/html/dev_guide/generated_cmake_api.html#catkin-package
# TODO: fill in what other packages will need to use this package
catkin_package(
    DEPENDS visualization_msgs moveit_msgs geometric_shapes rosconsole eigen
    CATKIN_DEPENDS # TODO
    INCLUDE_DIRS include ${catkin_INCLUDE_DIRS}# TODO include
    LIBRARIES ${catkin_LIBRARIES}# TODO
)
# include_directories(include ${Boost_INCLUDE_DIR} ${catkin_INCLUDE_DIRS})
# CATKIN_MIGRATION: removed during catkin migration
# cmake_minimum_required(VERSION 2.4.6)

# CATKIN_MIGRATION: removed during catkin migration
# include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)
#set(ROS_BUILD_TYPE Debug)
set(ROS_BUILD_TYPE RelWithDebInfo)


# CATKIN_MIGRATION: removed during catkin migration
# rosbuild_init()

find_package(Eigen REQUIRED)
include_directories(${EIGEN_INCLUDE_DIRS})

#set the default path for built executables to the "bin" directory
#set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
#set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

#common commands for building c++ executables and libraries
#add_library(${PROJECT_NAME} src/example.cpp)
#target_link_libraries(${PROJECT_NAME} another_library)
#
# CATKIN_MIGRATION: removed during catkin migration
# rosbuild_add_boost_directories()
#find_package(Boost REQUIRED COMPONENTS thread)
include_directories(${Boost_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS} include)
target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES} ${catkin_LIBRARIES})
#add_executable(example examples/example.cpp)
#target_link_libraries(example ${PROJECT_NAME})

add_library(distance_field
  src/distance_field.cpp
  src/propagation_distance_field.cpp
)

catkin_add_gtest(test/test_voxel_grid test/test_voxel_grid.cpp)
target_link_libraries(test/test_voxel_grid distance_field)

catkin_add_gtest(test/test_distance_field test/test_distance_field.cpp)
target_link_libraries(test/test_distance_field distance_field)

