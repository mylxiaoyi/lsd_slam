cmake_minimum_required(VERSION 2.4.6)
project(lsd_slam_viewer)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
find_package(catkin REQUIRED roscpp cv_bridge sensor_msgs std_msgs 
	message_generation dynamic_reconfigure roslib)

ADD_SUBDIRECTORY(${PROJECT_SOURCE_DIR}/thirdparty/Sophus)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${PROJECT_SOURCE_DIR}/cmake)

find_package(OpenCV REQUIRED)
find_package(Qt5 REQUIRED COMPONENTS OpenGL Xml)
set(QT_USE_QTOPENGL TRUE)
set(QT_USE_QTXML TRUE)
find_package(QGLViewer REQUIRED)
find_package(Eigen3 REQUIRED)
include_directories(
	${Qt5OpenGL_INCLUDE_DIRS}
	${Qt5Xml_INCLUDE_DIRS}
	${OpenCV_INCLUDE_DIRS}
	${EIGEN3_INCLUDE_DIR} ${QGLVIEWER_INCLUDE_DIR})


set(CMAKE_CXX_FLAGS
   "${SSE_FLAGS} -fPIC"
)

generate_dynamic_reconfigure_options(
	cfg/LSDSLAMViewerParams.cfg
)

add_message_files(
	FILES
	keyframeGraphMsg.msg
	keyframeMsg.msg
)

generate_messages(
	DEPENDENCIES
	std_msgs
)

catkin_package(CATKIN_DEPENDS message_runtime std_msgs)

# SSE Sources files
set(SOURCE_FILES         
  src/PointCloudViewer.cpp
  src/KeyFrameDisplay.cpp
  src/KeyFrameGraphDisplay.cpp
  src/settings.cpp
)

set(HEADER_FILES     
  src/PointCloudViewer.h
  src/KeyFrameDisplay.h
  src/KeyFrameGraphDisplay.h
  src/settings.h
)

include_directories(
  ${PROJECT_SOURCE_DIR}/thirdparty/Sophus
)  

add_executable(viewer src/main_viewer.cpp ${SOURCE_FILES} ${HEADER_FILES})

target_link_libraries(viewer ${catkin_LIBRARIES} Qt5::OpenGL Qt5::Xml ${OpenCV_LIBS} -lQGLViewer -lGL -lpthread)

add_executable(videoStitch src/main_stitchVideos.cpp)

target_link_libraries(videoStitch ${OpenCV_LIBS})
