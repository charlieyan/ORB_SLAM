cmake_minimum_required(VERSION 2.4.6)
project(orb_slam)

## Compile as C++11, supported in ROS Kinetic and newer
add_compile_options(-std=c++11)

# include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# rosbuild_init()

# IF(NOT ROS_BUILD_TYPE)
#   SET(ROS_BUILD_TYPE Release)
# ENDIF()

MESSAGE("Build type: " ${ROS_BUILD_TYPE})

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall  -O3 -march=native ")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall  -O3 -march=native")

set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)

LIST(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake_modules)

# 2019-06-16 ADDED
list(APPEND CMAKE_PREFIX_PATH
  "/usr/local/Cellar/opencv@3/3.4.5_2/share/OpenCV/")

find_package(OpenCV REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(Boost REQUIRED)

include_directories(
  ${PROJECT_SOURCE_DIR}
  ${EIGEN3_INCLUDE_DIR}
  include/

  /usr/local/Cellar/boost/1.70.0/include/ # 2019-06-16 ADDED
  /opt/ros/lunar/include/ # 2019-06-16 ADDED
  /usr/local/Cellar/g2o/2017-07-30/include/ # 2019-06-20 ADDED
)

add_executable(${PROJECT_NAME}
  src/main.cc
  src/Tracking.cc
  src/LocalMapping.cc
  src/LoopClosing.cc
  src/ORBextractor.cc
  src/ORBmatcher.cc
  src/FramePublisher.cc

  src/Converter.cc

  src/MapPoint.cc
  src/KeyFrame.cc
  src/Map.cc
  src/MapPublisher.cc
  src/Optimizer.cc
  src/PnPsolver.cc
  src/Frame.cc
  src/KeyFrameDatabase.cc
  src/Sim3Solver.cc
  src/Initializer.cc
)

# rosbuild_add_boost_directories()
# rosbuild_link_boost(${PROJECT_NAME} thread)

target_link_libraries(${PROJECT_NAME}
${OpenCV_LIBS}
${EIGEN3_LIBS}
${PROJECT_SOURCE_DIR}/Thirdparty/DBoW2/lib/libDBoW2.dylib # libDBoW2.so, # 2019-06-20 in osx, compiled targets are dylibs!!!
# ${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o.so
# 2019-06-20 brew install brewsci/science/g2o on mac, not from Thirdparty!
/usr/local/lib/libg2o_core.dylib
)

