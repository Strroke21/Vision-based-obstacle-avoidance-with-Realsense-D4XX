# Introduction
In this blog post, we will attempt to extend ArduPilot’s obstacle avoidance ability 26 with the Intel Realsense depth camera.
# Prerequisites 
**Harware**
- Onboard computer with at least one USB 3.0 port.
- x86-based CPU is recommended for compatibility with Intel’s libraries and hardware. I am using the Up Squared SoC board 58. For other boards of other architecture (RPi, Jetson etc.), you might encounter troubles with installation the packages below.
- Since we are processing depth images to some degree, it might be the case the processing speed will vary greatly between the RPi4, Up2, Jetson Nano/TX/Xavier etc. So keep that in mind to tune the system if the performance is not satisfactory to you.
Sensor: we are using the Realsense D435 depth camera 35, but presumably you can also use the program with the D415 or D435i without any modifications (except for general settings).
The camera is assumed to be facing forward. For other orientations, you need to change the code, but the process is straightforward, as will be explain in the discussion section.

**Vehicle:**
Any DIY copter 20 of your choosing. The content of this post has been tested with copter. For rover, the AP’s parameters should be changed following the corresponding wikis 10.
Configuration: rangefinder 15 and optical flow 13 sensors would be necessary for stable flight. I am using the TeraRanger One 21 and PX4Flow 19.

**Software**
OS: Ubuntu 18.04. Highly recommended as this release is the most up-to-date with the required libraries.
Please note that we will provide support exclusively on this version. For earlier version of Ubuntu, you need to modify each installation step to work for your case (in case of too many conflicting installations, you may find Anaconda 8 to be helpful).
Python 3.6 and above, which is also the standard for Ubuntu 18.04. Check with $ python3 -V, you should see the version, something like Python 3.6.9.
librealsense 12: download or install from source.

# Installation
Installation of supporting packages:
>>pip3 install pyrealsense2
>>
>>pip3 install transformations
>>
>>pip3 install dronekit
>>
>>pip3 install apscheduler
>>
>>pip3 install pyserial # For serial connection
>>
>>pip3 install opencv-python

- Only necessary if you installed the minimal version of Ubuntu
sudo apt install python3-opencv
Download the script, or clone the ROS node if you are using ROS (although we won’t cover ROS in this blog post):
- Clone the package in the ROS workspace or any location if you don't use ROS

>>cd ~/catkin_ws/src
>>
>>git clone https://github.com/hoangthien94/vision_to_mavros.git
>>
>>cd vision_to_mavros/script
>>
>>chmod +x d4xx_to_mavlink.py
>>
>>chmod +x opencv_depth_filtering.py
>>
The main script to be used with AP is d4xx_to_mavlink.py. The second script, opencv_depth_filtering.py is used to test out different filtering options, which is explained in more details in the final discussion section.

# Parameter Configuration
- 1. Companion Computer Side
Within the script d4xx_to_mavlink.py, find the following parameters and reconfigure them if need to:

Connection to the flight controller:
- Default configurations for connection to the FCU
>>connection_string_default = '/dev/ttyUSB0'
>>
>>connection_baudrate_default = 921600
General settings for the depth camera, check the datasheet 5 to make sure your combination of w x h x fps is supported:
>>WIDTH  = 640              # Defines the number of columns for each frame or zero for auto resolve
>>HEIGHT = 480              # Defines the number of lines for each frame or zero for auto resolve
>>FPS    = 30               # Defines the rate of frames per second
