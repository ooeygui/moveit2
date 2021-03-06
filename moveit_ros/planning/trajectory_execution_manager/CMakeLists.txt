set(MOVEIT_LIB_NAME moveit_trajectory_execution_manager)

add_library(${MOVEIT_LIB_NAME} SHARED src/trajectory_execution_manager.cpp)
set_target_properties(${MOVEIT_LIB_NAME} PROPERTIES VERSION "${${PROJECT_NAME}_VERSION}")
set_target_properties(${MOVEIT_LIB_NAME} PROPERTIES WINDOWS_EXPORT_ALL_SYMBOLS TRUE)
ament_target_dependencies(${MOVEIT_LIB_NAME}
  moveit_core
  moveit_ros_occupancy_map_monitor
  rclcpp
  Boost
  tinyxml_vendor
  pluginlib
  std_msgs
  sensor_msgs
  moveit_msgs
  tf2_eigen
)
target_link_libraries(${MOVEIT_LIB_NAME}
  moveit_planning_scene_monitor
)

install(TARGETS ${MOVEIT_LIB_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

install(DIRECTORY include/ DESTINATION include)

add_library(test_controller_manager_plugin SHARED test/test_moveit_controller_manager_plugin.cpp)
set_target_properties(test_controller_manager_plugin PROPERTIES VERSION "${${PROJECT_NAME}_VERSION}")
set_target_properties(test_controller_manager_plugin PROPERTIES WINDOWS_EXPORT_ALL_SYMBOLS TRUE)
ament_target_dependencies(test_controller_manager_plugin
  rclcpp
  Boost
  pluginlib
)
target_link_libraries(test_controller_manager_plugin ${MOVEIT_LIB_NAME})

add_executable(test_controller_manager test/test_app.cpp)
ament_target_dependencies(test_controller_manager
  rclcpp
  Boost
  pluginlib
)
target_link_libraries(test_controller_manager ${MOVEIT_LIB_NAME})

install(TARGETS test_controller_manager
  DESTINATION lib/${PROJECT_NAME}
)
