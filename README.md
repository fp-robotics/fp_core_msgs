![alt text](https://www.fp-robotics.com/wp-content/themes/fprobot/img/fp-robotics_logo.png "F&P Robotics AG")

# F&P CORE MESSAGES

Author: Martin Möller (MPM)
Compatible with myP version 1.4.0.0

Message and service types to read data from the myP core and access selected functions through ROS services.

## Installation

Clone this repo into the `src` directory of your catkin workspace and build it, either through `catkin_make` or `catkin build`. Certain add-ons of myP advertise their own services and therefore require additional message and service types. Please have a look at these repos:

__**TODO: MPM list other msg repos here.**__

## Setting up ROS environment

Before accessing myP functions through ROS, you first need to source your ROS environment. Currently, a P-Rob starts and connects to its own roscore. Thus, you need to set your `ROS_MASTER_URI` to the IP-address of the P-Rob (choose the dynamic port, if your robot is hardware version D or higher).

In a next step, we are planning to design this setup more flexible, by adding an interface to dynamically change the ROS settings, such that you can use a roscore on another device. For the moment, however, sourcing your ROS environment looks similar to this:

```bash 
source /opt/ros/kinetic/setup.bash
source <PATH TO CATKIN-WS>/devel/setup.bash
export ROS_MASTER_URI=http://<P-ROB DYNAMIC IP>:11311
export ROS_IP=<YOUR DYNAMIC IP>
```

## Reading Data from topics

You can now view the available ROS topics with the command `rostopic list`. Please note that myP only starts publishing data upon connecting. All topics are being advertised under a namespace, whose name is equal to your type of robot (e.g. `/PRob2R`).

```bash
/InformationPoolInput
/PRob2R/digital_inputs
/PRob2R/digital_outputs
/PRob2R/joint_currents
/PRob2R/joint_positions
/PRob2R/joint_states
/PRob2R/joint_velocities
/PRob2R/pose
/PRob2R/pose_alternative
/PRob2R/sensors
/PRob2R/transform
/node_monitor_status
/rosout
/rosout_agg
```

Unless your are operating an F&P service robot (e.g. Lio), please ignore the topics `/InformationPoolInput` and `/node_monitor_status`. The topics `/PRob2R/pose`, `/PRob2R/pose_alternative` and `/PRob2R/transform` merely use different message types to publish the robot's pose. The content of the remaining topics should be clear from their names.

## Calling myP functions through services

A selection of myP functions is available as ROS services. To view them, run `rosservice list`. The outer namespace is the same as for the ROS topics, e.g. `/PRob2R`. Then follows a sub-namespace for the myP core and all available add-ons. For instance, if you have purchased the add-ons `learning` and `task_manager`, the result will look similar to this (note the services for `recognize_object` and `play_task` towards the end):

```bash
/InformationPoolGet
/PRob2R/core/calibrate
/PRob2R/core/change_project
/PRob2R/core/close_gripper
/PRob2R/core/connect
/PRob2R/core/disconnect
/PRob2R/core/forward_kinematics
/PRob2R/core/get_grid_points
/PRob2R/core/get_status
/PRob2R/core/hold
/PRob2R/core/inverse_kinematics
/PRob2R/core/is_motor_moving
/PRob2R/core/log_message
/PRob2R/core/move_joint
/PRob2R/core/move_to_pose
/PRob2R/core/move_tool
/PRob2R/core/open_gripper
/PRob2R/core/pause
/PRob2R/core/play_path
/PRob2R/core/raise_error
/PRob2R/core/raise_warning
/PRob2R/core/read_actuator_current
/PRob2R/core/read_actuator_position
/PRob2R/core/read_digital_inputs
/PRob2R/core/read_digital_outputs
/PRob2R/core/read_gripper_angle
/PRob2R/core/read_sensor_data
/PRob2R/core/read_tcp_pose
/PRob2R/core/release
/PRob2R/core/resume
/PRob2R/core/run_simple_path
/PRob2R/core/stop
/PRob2R/core/test_script
/PRob2R/core/wait_for_motor
/PRob2R/core/write_digital_outputs
/PRob2R/learning/recognize_object
/PRob2R/task_manager/play_task
/PRob2R__myP__v1_4_0_1/get_loggers
/PRob2R__myP__v1_4_0_1/set_logger_level
/rosout/get_loggers
/rosout/set_logger_level
```

Again, unless you are operating a service robot like Lio, you can safely ignore the service `/InformationPoolGet`.

## Examples

Before any other services become available, you need to connect to myP.

```bash
rosservice call /PRob2R/core/connect "robot_kind: 'Connect to Robot'
robot_name: 'PRob2R'" 
success: True
message: ''
```
Should you not be able to establish a connection for whatever reason, you can find out about it through the success flag and the return value message. For instance, if you try to execute a script when the robot is not yet connected, the return message will look like this:

```bash
rosservice call /PRob2R/core/test_script "code: 'print(\"hello world\")'
type: 'main'
id: 0" 
success: False
message: "Could not execute the action 'test_script'. The robot needs to be connected."
```

Once you are connect, the success flag will return `true`. Next, please calibrate your robot, if you haven't already.

```bash
rosservice call /PRob2R/core/calibrate "script_id: 0
script_name: ''" 
success: True
message: ''
```

Script ID 0 with no script name specified simply selects the default calibration.

Make sure that you are in the right project:

```bash
rosservice call /PRob2R/core/change_project "value: 'Default'" 
success: True
message: ''
```

You can also retrieve information about the state of the robot through ROS services. For instance:

```bash
rosservice call /PRob2R/core/read_tcp_pose 
pose: 
  position: 
    x: 0.0
    y: 0.0
    z: 1378.5
  orientation: 
    phi: 0.0
    theta: 90.0
    psi: 180.0
success: True
message: ''
```

When you use myP's ROS interface through the command line, please be careful with the default values. Since ROS autocompletes with the default values for each respective message type, the result might not be what you want. For example, if you start typing the command for `move_joint` and then just auto-complete the message, this is what happens:

```bash
rosservice call /PRob2R/core/move_joint "actuator_ids: [0]
position: [0]
velocity: 0.0
acceleration: 0.0
block: false
relative: false" 
success: False
message: "An actuator with ID ['0'] does not exist."
```

Thus you need to think about each argument and actively set it to the desired value. For instance, to move joint 6 to an absolute position of 90 degrees, this is probably what you want to see:

```bash
rosservice call /PRob2R/core/move_joint "actuator_ids: [6]
position: [90]
velocity: 20.0
acceleration: 100.0
block: true 
relative: false" 
success: True
message: ''
```

Similarly for `move_tool`, when a P-Grip is mounted in the 180-degree-configuration (note that the argument `frame` needs to be either `'base'` or `'tool'`):

```bash
rosservice call /PRob2R/core/move_tool "x: 0.0
y: 0.0
z: 1200.0
orientation: [0, 90, 180]
velocity: 20.0
acceleration: 100.0
block: true 
relative: false
frame: 'base'" 
success: True
message: ''
```

When you're done, don't forget to disconnect.

```bash
rosservice call /PRob2R/core/disconnect
success: True
message: ''
```

## Expected Changes

The ROS connection in myP 1.4 is the second iteration of myP's ROS interface. To further enhance its usability and be compatible with all the features that ROS provides, we still expect a few further modifications in the future. Here is a list of such modifications, which might still break backwards-compatibility, but from which we expect large usability-improvements:


## Known Issues

- calibrate requires ID of calibration script. To find the ID, you need to download and open the database, e.g. with SQLiteBrowser.
- At the moment, gripper functions are advertised under the namespace `/PRob2R/core` instead of `/PRob2R/pgrip`, although they are running inside the add-on `pgrip` (currently free of charge). This will change, once we revamp `pgrip`, such that its structure is consistent with the other add-ons of myP (it's a special case at the moment). Please be aware that you will then have to call those services under the new namespace.
- Essentially, the argument `robot_name` of the `connect`-service has the only effect of renaming your database, e.g. `Giggity_database.db` instead of `PRob2R_databse.db`. To avoid any confision, please make sure to always call it with `robot_name: 'PRob2R'`.

## Support

Should you encounter any issues not listed above, please contact us under [support@fp-robotics.com](support@fp-robotics.com).

