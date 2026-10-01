cmake_minimum_required(VERSION 3.0.2)
project(erp2026_msgs)

find_package(catkin REQUIRED COMPONENTS
  message_generation
)

add_message_files(
  FILES
  ModeCmd.msg
  DriveCmd.msg
  SerialFeedBack.msg
)

generate_messages()

catkin_package(
  CATKIN_DEPENDS message_runtime
)
