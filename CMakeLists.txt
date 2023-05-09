cmake_minimum_required(VERSION 2.4.6)
project(simple_mockup_description)

find_package(mc_rtc REQUIRED)

option(DISABLE_ROS "Do not attempt to find catkin" OFF)

if(NOT DISABLE_ROS)
  find_package(catkin)
else()
  set(catkin_FOUND FALSE)
endif()

if(catkin_FOUND)

  catkin_package()

  set(SIMPLE_MOCKUP_MESHES_LOCATION "package://${PROJECT_NAME}/meshes")

else()

  set(CATKIN_PACKAGE_SHARE_DESTINATION share/mc_rtc/${PROJECT_NAME})

  include(CMakePackageConfigHelpers)
  set(VERSION_CONFIG "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake")
  set(PROJECT_CONFIG "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake")
  set(CONFIG_INSTALL_DIR "lib/cmake/${PROJECT_NAME}")
  write_basic_package_version_file(
    ${VERSION_CONFIG}
    VERSION 1.0.0
    COMPATIBILITY SameMajorVersion
  )
  configure_package_config_file("${CMAKE_CURRENT_SOURCE_DIR}/cmake/Config.cmake.in" "${PROJECT_CONFIG}" INSTALL_DESTINATION "${CONFIG_INSTALL_DIR}")
  install(FILES "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}Config.cmake" "${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake" DESTINATION "${CONFIG_INSTALL_DIR}")

  set(SIMPLE_MOCKUP_MESHES_LOCATION "file://${CMAKE_INSTALL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}/meshes")
endif()

set(ALIAS_OUT "${CMAKE_CURRENT_BINARY_DIR}/aliases/simple_mockup.yaml")
set(SIMPLE_MOCKUP_LOCATION "${CMAKE_CURRENT_SOURCE_DIR}")
set(SIMPLE_MOCKUP_INSTALL_LOCATION "${CMAKE_INSTALL_PREFIX}/${CATKIN_PACKAGE_SHARE_DESTINATION}")
configure_file(aliases/simple_mockup.in.yaml "${ALIAS_OUT}")

set(LOCAL_ALIAS "$ENV{HOME}/.config/mc_rtc/aliases/simple_mockup.yaml")
configure_file(aliases/simple_mockup.in.yaml "${LOCAL_ALIAS}")

set(LOCAL_MODULE "$ENV{HOME}/.config/mc_rtc/modules/simple_mockup.yaml")
configure_file(modules/simple_mockup.in.yaml "${SIMPLE_MOCKUP_INSTALL_LOCATION}/modules/simple_mockup.yaml")
