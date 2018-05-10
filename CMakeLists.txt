cmake_minimum_required(VERSION 2.8.3)
project(fastslam)

# SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -O3")
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -fPIC -g -rdynamic -msse4")

# search for everything we need to build the package
find_package(catkin REQUIRED COMPONENTS
  roscpp
  sensor_msgs
  dvs_msgs
  geometry_msgs
  visualization_msgs
  cv_bridge
  image_transport
  cmake_modules
  tf
  qt_build
)

##############################################################################
# Qt Environment
##############################################################################

set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
#SET(CMAKE_AUTOUIC ON)

find_package(Qt5 REQUIRED
    Core
    Gui
    Widgets
    PrintSupport
)

##############################################################################
# Sections
##############################################################################

file(GLOB QT_FORMS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} ui/*.ui)
file(GLOB QT_RESOURCES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} resources/*.qrc)


QT5_ADD_RESOURCES(QT_RESOURCES_CPP ${QT_RESOURCES})
QT5_WRAP_UI(QT_FORMS_HPP ${QT_FORMS})

find_package(catkin_simple REQUIRED)

# since we need eigen and boost search them as well
# find_package makes the ${..._INCLUDE_DIRS} ${..._LIBRARIES} variables we use later
find_package(OpenCV REQUIRED)
find_package(Eigen3 REQUIRED)

# tell catkin where to find the headers for this project
include_directories(
  ${catkin_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIRS}

  ${Qt5Core_INCLUDE_DIRS}
  ${Qt5Gui_INCLUDE_DIRS}
  ${Qt5Widgets_INCLUDE_DIRS}
  ${Qt5PrintSupport_INCLUDE_DIRS}
)


catkin_simple()

# make the executable
# cs_add_executable(fastslam
#   ./src/main.cpp
# )

cs_add_executable(fastslam
  ./src/main.cpp
  ./src/fastslam_1.cpp
  ./src/fastslam_2.cpp
  ./src/ekfslam_1.cpp
  ./src/fastslam_core.cpp
  ./src/SLAM_Thread.cpp
  ./src/SLAM_Plot.cpp
  ./src/utils.cpp
)

# link the executable to the necesarry libs
target_link_libraries(fastslam
    ${catkin_LIBRARIES}
    ${OpenCV_LIBRARIES}
    ${QT_LIBRARIES}
)