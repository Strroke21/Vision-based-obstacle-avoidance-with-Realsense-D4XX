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
librealsense 12: download or install from source 13.
