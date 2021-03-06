cmake_minimum_required(VERSION 2.8.3)
project(oni_vicon_calibration)

find_package(catkin REQUIRED COMPONENTS
    roscpp
    std_msgs
    message_generation
    actionlib_msgs
    actionlib
    interactive_markers
    simple_object_tracker
    tf
    oni_vicon_common)

find_package(Boost REQUIRED)
include_directories(${Boost_INCLUDE_DIRS})

find_package(Eigen REQUIRED)
include_directories(${Eigen_INCLUDE_DIRS})

################################################
## Declare ROS messages, services and actions ##
################################################
add_action_files(
    DIRECTORY action
    FILES
    GlobalCalibration.action
    LocalCalibration.action
    TestCalibration.action
)

add_service_files(
    DIRECTORY srv
    FILES
    LoadGlobalCalibration.srv
    LoadLocalCalibration.srv
    SaveGlobalCalibration.srv
    SaveLocalCalibration.srv
    ContinueLocalCalibration.srv
    CompleteLocalCalibration.srv
    ContinueGlobalCalibration.srv
    CompleteGlobalCalibration.srv
    ContinueTestCalibration.srv
)

generate_messages(
    DEPENDENCIES
    std_msgs actionlib_msgs std_msgs visualization_msgs geometry_msgs
)

###################################
## catkin specific configuration ##
###################################
catkin_package(
    INCLUDE_DIRS include
    LIBRARIES oni_vicon_calibration
    CATKIN_DEPENDS
        actionlib_msgs
        actionlib
        interactive_markers
        simple_object_tracker
        tf
        oni_vicon_common
    DEPENDS eigen
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(
    include
    ${catkin_INCLUDE_DIRS}
    "/usr/include/ni/"
)

set(oni_vicon_calibration_HEADERS
    include/${PROJECT_NAME}/calibrator.hpp
)

set(oni_vicon_calibration_SOURCES
    src/${PROJECT_NAME}/calibrator.cpp
)

add_library(oni_vicon_calibration
   ${oni_vicon_calibration_HEADERS}
   ${oni_vicon_calibration_SOURCES}
)

target_link_libraries(oni_vicon_calibration
  ${catkin_LIBRARIES}
  ${Boost_LIBRARIES}
  yaml-cpp
)
#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark executables and/or libraries for installation
# install(TARGETS oni_vicon_calibration oni_vicon_calibration_node
#   ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark cpp header files for installation
# install(DIRECTORY include/${PROJECT_NAME}/
#   DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
#   FILES_MATCHING PATTERN "*.h"
#   PATTERN ".svn" EXCLUDE
# )

## Mark other files for installation (e.g. launch and bag files, etc.)
# install(FILES
#   # myfile1
#   # myfile2
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

#############
## Testing ##
#############

## Add gtest based cpp test target and link libraries
# catkin_add_gtest(${PROJECT_NAME}-test test/test_oni_vicon_calibration.cpp)
# if(TARGET ${PROJECT_NAME}-test)
#   target_link_libraries(${PROJECT_NAME}-test ${PROJECT_NAME})
# endif()

## Add folders to be run by python nosetests
# catkin_add_nosetests(test)
