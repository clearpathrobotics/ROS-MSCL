## Description

Interface (driver) software, including ROS node, for inertial sensors compatible with the [Microstrain Communication Library (MSCL)](https://github.com/LORD-MicroStrain/MSCL).

MSCL is developed by [LORD Sensing - Microstrain](http://microstrain.com) in Williston, VT.


## Build Instructions

#### MSCL
MSCL pre-built packages can be found here: [MSCL Packages](https://github.com/LORD-MicroStrain/MSCL#downloads)

Install instructions can be found here: [How to Use MSCL](https://github.com/LORD-MicroStrain/MSCL/blob/master/HowToUseMSCL.md#linux)

If you choose to install MSCL at a location other than /usr/share, [CMakeLists.txt](https://github.com/LORD-MicroStrain/ROS-MSCL/blob/master/CMakeLists.txt) will need to be updated with the install path.

#### Building from source
1. Install ROS and create a workspace: [Installing and Configuring Your ROS Environment](http://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment)

2. Clone ros_mscl into the your_workspace/src folder.

3. Locate and register the ros_mscl package: `rospack find ros_mscl`

4. Build your workspace:

        cd ~/your_workspace
        catkin_make
        source ~/your_workspace/devel/setup.bash
   The source command may need to be run in each terminal prior to launching a ROS node.
#### Launch the node and publish data
The following command will launch the driver. Keep in mind each instance needs to be run in a separate terminal.

        roslaunch ros_mscl microstrain.launch
Optional launch parameters:
- name: namespace the node will publish messages to, default: gx5
- port: serial port name to connect to the device over, default: /dev/ttyACM0
- baudrate: baud rate to open the connection with, default: 115200
- imu_rate: sample rate for IMU data (hz), default: 100
- debug: output debug info? default: false
- diagnostics: output diagnostic info? default: true

To check published topics:

    rostopic list

**Example**: Connect to and publish data from two devices simultaneously
In two different terminals:

    roslaunch ros_mscl microstrain.launch name:=sensor1234

    roslaunch ros_mscl microstrain.launch name:=bestSensor port:=/dev/ttyACM1
This will launch two nodes that publish data to different namespaces:
- sensor1234, connected over port: /dev/ttyACM0
- bestSensor, connected over port: /dev/ttyACM1

An example subscriber node can be found here: [ROS-MSCL Examples](https://github.com/LORD-MicroStrain/ROS-MSCL/tree/master/Examples)


## Troubleshooting
If you encounter of the form

    /opt/ros/melodic/lib/ros_mscl/ros_mscl_node: error while loading shared libraries: libmscl.so: cannot open shared object file: No such file or directory

check that the directory you installed mscl.so to is part of your 
LD_LIBRARY_PATH.  By default this is ``/usr/share/c++-mscl``.

Running

    export LD_LIBRARY_PATH=/usr/share/c++-mscl:$LD_LIBRARY_PATH

may be necessary before launching the ros_mscl node.


## License
ROS-MSCL is released under the MIT License - see the `LICENSE` file in the source distribution.

Copyright (c)  2020, Parker Hannifin Corp.
