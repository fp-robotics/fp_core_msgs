cmake_minimum_required(VERSION 2.8.3)
project(fp_core_msgs)

# find message_generation library
find_package(catkin REQUIRED
  geometry_msgs
  message_generation
)

# generate messages in the 'msg' folder
add_message_files(
  FILES
  JointAngles.msg
  OrientationNautical.msg
  Pose.msg
)

# generate services in the 'srv' folder
add_service_files(
  FILES
  ForwardKinematics.srv
  GetGridPoints.srv
  GetListBool.srv
  GetListFloat.srv
  GetPose.srv
  GetStatus.srv
  Initialize.srv
  InverseKinematics.srv
  IsMotorMoving.srv
  ListInt.srv
  MoveJoint.srv
  MoveTool.srv
  RequestData.srv
  SetInt.srv
  SetString.srv
  SimplePath.srv
  TestScript.srv
  WriteAnalogIO.srv
  WriteDigitalIO.srv
)

# generate added messages and services
generate_messages(
  DEPENDENCIES
  geometry_msgs
)

