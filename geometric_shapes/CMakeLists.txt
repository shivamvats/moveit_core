# Catkin User Guide: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/user_guide.html
# Catkin CMake Standard: http://www.ros.org/doc/groovy/api/catkin/html/user_guide/standards.html
cmake_minimum_required(VERSION 2.8.3)
project(geometric_shapes)
# Load catkin and all dependencies required for this package
# TODO: remove all from COMPONENTS that are not catkin packages.
find_package(catkin REQUIRED COMPONENTS rosconsole resource_retriever visualization_msgs random_numbers shape_msgs shape_tools libqhull assimp eigen octomap console_bridge)

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

set(ROS_BUILD_TYPE Release)


# CATKIN_MIGRATION: removed during catkin migration
# rosbuild_init()

find_package(Eigen REQUIRED)
include_directories(${EIGEN_INCLUDE_DIRS})

find_package(PkgConfig REQUIRED)
pkg_check_modules(OCTOMAP REQUIRED octomap)
include_directories(${OCTOMAP_INCLUDE_DIRS})
link_directories(${OCTOMAP_LIBRARY_DIRS})

find_package(ASSIMP QUIET)
if (NOT ASSIMP_FOUND)
  pkg_check_modules(ASSIMP assimp)
endif()
if (ASSIMP_FOUND)
  if( ${ASSIMP_VERSION} STRGREATER "2.0.0" )
    set(IS_ASSIMP3 1)
    add_definitions(-DIS_ASSIMP3)
    message(STATUS "Found assimp v3")
  else()
    message(STATUS "Found assimp v2")
  endif()
  include_directories(${ASSIMP_INCLUDE_DIRS})
  link_directories(${ASSIMP_LIBRARY_DIRS})
else()
  message(STATUS "could not find assimp (perhaps available thorugh ROS package?), so assimping assimp v2")
  set(ASSIMP_LIBRARIES assimp)
  set(ASSIMP_LIBRARY_DIRS)
  set(ASSIMP_CXX_FLAGS)
  set(ASSIMP_CFLAGS_OTHER)
  set(ASSIMP_LINK_FLAGS)
  set(ASSIMP_INCLUDE_DIRS)
  set(IS_ASSIMP3 0) # most likely not
endif()

find_library(QHULL_LIB qhull)


# CATKIN_MIGRATION: removed during catkin migration
# rosbuild_add_boost_directories()

#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

add_library(${PROJECT_NAME}
  src/shapes.cpp
  src/shape_operations.cpp
  src/bodies.cpp
  src/body_operations.cpp)
target_link_libraries(${PROJECT_NAME} ${ASSIMP_LIBRARIES} ${OCTOMAP_LIBRARIES} ${QHULL_LIB})


# Unit tests
catkin_add_gtest(test_point_inclusion test/test_point_inclusion.cpp)
target_link_libraries(test_point_inclusion ${PROJECT_NAME})
find_package(Boost REQUIRED COMPONENTS system filesystem)
include_directories(${Boost_INCLUDE_DIRS})
target_link_libraries(test_point_inclusion ${Boost_LIBRARIES})
## Generate added messages and services with any dependencies listed here
#generate_messages(
#    #TODO DEPENDENCIES geometry_msgs std_msgs
#)

# catkin_package parameters: http://ros.org/doc/groovy/api/catkin/html/dev_guide/generated_cmake_api.html#catkin-package
# TODO: fill in what other packages will need to use this package
catkin_package(
    DEPENDS rosconsole resource_retriever visualization_msgs random_numbers shape_msgs shape_tools libqhull assimp eigen octomap console_bridge
    CATKIN_DEPENDS # TODO
    INCLUDE_DIRS # TODO include
    LIBRARIES # TODO
)