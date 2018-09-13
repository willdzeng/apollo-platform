cmake_minimum_required(VERSION 2.8.3)

project(audio_capture)

find_package(catkin REQUIRED COMPONENTS roscpp audio_common_msgs)

find_package(PkgConfig)
pkg_check_modules(GST gstreamer-0.10 REQUIRED)

find_package(Boost REQUIRED COMPONENTS thread)

include_directories(${catkin_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS} ${GST_INCLUDE_DIRS})

catkin_package(
  LIBRARIES audio_capture ${Boost_LIBRARIES}
  CATKIN_DEPENDS roscpp audio_common_msgs
  # INCLUDE_DIRS include
)

add_executable(audio_capture src/audio_capture.cpp)
add_dependencies(audio_capture roscpp ${catkin_EXPORTED_TARGETS})
target_link_libraries(audio_capture ${catkin_LIBRARIES} ${GST_LIBRARIES} ${Boost_LIBRARIES} -lrt)

install(TARGETS audio_capture audio_capture
ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

install(DIRECTORY launch
   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})