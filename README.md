![alt text](https://www.fp-robotics.com/wp-content/themes/fprobot/img/fp-robotics_logo.png "F&P Robotics AG")

# F&P CORE MESSAGES

Author: Martin Möller (MPM)  
Compatible with myP versions 1.4.0.(0-1)

Message and service types to read data from the myP core and access selected functions through ROS services.

## Installation

Clone this repo into the `src` directory of your catkin workspace and build it, either through `catkin_make` or `catkin build`. Certain add-ons of myP advertise their own services and therefore require additional message and service types. Please have a look at these repos:

- [fp_pgrip_msgs](https://github.com/fp-robotics/fp_pgrip_msgs) (if you want to control a P-Grip via ROS in addition)
- [fp_learning_msgs](https://github.com/fp-robotics/fp_learning_msgs) (if you are using the myP Learning Module in addition)

## Using myP's ROS module

Please have a look at the __**myP ROS Connection Module 1.4.0**__ manual for detailed examples.  
Should you find any issues, please report them to [support@fp-robotics.com](support@fp-robotics.com).

