cmake_minimum_required(VERSION 2.8.3)

project(gscam)

# System Dependencies
find_package(PkgConfig)

  message(STATUS "gst 1.0")
  pkg_check_modules(GSTREAMER REQUIRED gstreamer-1.0)
  pkg_check_modules(GST_APP REQUIRED gstreamer-app-1.0)


  # Use Catkin
  find_package(catkin REQUIRED 
    COMPONENTS roscpp image_transport sensor_msgs nodelet
    camera_calibration_parsers camera_info_manager
    )

  catkin_package(
    INCLUDE_DIRS include
    LIBRARIES gscam
    CATKIN_DEPENDS roscpp nodelet image_transport sensor_msgs
    camera_calibration_parsers camera_info_manager
    DEPENDS GSTREAMER GST_APP
    )

  include_directories(
    include
    ${catkin_INCLUDE_DIRS}
    ${GLIB_INCLUDE_DIRS}
    ${GST_APP_INCLUDE_DIRS})

  add_library(gscam src/gscam.cpp)
  target_link_libraries(gscam
    ${catkin_LIBRARIES}
    ${GSTREAMER_LIBRARIES}
    ${GST_APP_LIBRARIES})

  add_executable(gscam_node src/gscam_node.cpp)
  target_link_libraries(gscam_node gscam
    ${catkin_LIBRARIES}
    ${GSTREAMER_LIBRARIES}
    ${GST_APP_LIBRARIES})
  set_target_properties(gscam_node PROPERTIES OUTPUT_NAME gscam)

