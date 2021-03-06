cmake_minimum_required(VERSION 2.8.3)
project(movingtracker)

add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)# required for YCM
find_package(catkin REQUIRED COMPONENTS
  roscpp
  std_msgs
  tf
  nav_msgs
  sensor_msgs
  cmake_modules REQUIRED
  message_generation
)

find_package(Eigen REQUIRED)
find_package(OpenMP REQUIRED)

#MultiThreading Library
OPTION (USE_OpenMP "Use OpenMP" OFF)
IF(USE_OpenMP)
  FIND_PACKAGE(OpenMP)
  IF(OPENMP_FOUND)
    #SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
    #SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fopenmp")
  ENDIF()
ENDIF()

add_message_files(
  FILES
  Track.msg
  TrackArray.msg
)

generate_messages(
  DEPENDENCIES
  nav_msgs
  std_msgs
)

catkin_package(
 CATKIN_DEPENDS roscpp std_msgs message_runtime
)

include_directories(
# include
  ${catkin_INCLUDE_DIRS}
  ${Eigen_INCLUDE_DIRS}
)

add_executable(movingtracker src/main.cpp src/movingtracker.cpp src/l_shape_tracker.cpp src/kalman-cpp/kalman.cpp src/cluster.cpp)
add_dependencies(movingtracker ${catkin_EXPORTED_TARGETS})
target_link_libraries(movingtracker ${catkin_LIBRARIES})
