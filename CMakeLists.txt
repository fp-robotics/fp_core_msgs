cmake_minimum_required(VERSION 2.8.3)
project(fp_core_msgs)

# find message_generation library
find_package(catkin REQUIRED
  message_generation
)

# generate services in the 'srv' folder
add_service_files(
  FILES
  Calibrate.srv
  DigitalInputOutput.srv
  GetStatus.srv
  Initialize.srv
  SetInt.srv
  MoveJoint.srv
  MoveTool.srv
  SetString.srv
  SimplePath.srv
  TestScript.srv
)

# generate added messages and services
generate_messages()

