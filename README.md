# pmd dvrk project
**A compatible dvrk controller based on PMD Prodigy/CME machine controller.** This repository only contains documentation and links to the components.

# overview
The PMD machine controller is a complete motor control solution built with an ARM microcontroller to run user code, an FPGA to handle IO, and motor amplifiers (Atlas). The Atlas has a microcontroller for software current control and three half bridges. Because the current control runs in software (instead of hardware analog circuit with low-pass filter in QLA), you get higher bandwidth and tuneable current control performance.

I wanted to make this PMD controller a drop-in replacement for the QLA+firewire controller. Unfortunately, [cisst-saw](https://github.com/jhu-cisst/cisst-saw) was not written with an alternative motor controller in mind. For now, part of this project exists as a fork of cisst-saw, which means the PMD-based controller cannot co-exist with the QLA+firewire-based controller. 

I made an adapter board that sits on top of the machine controller.

![](block.png)

I removed the code in `sawRobotIO1394` that talks to the firewire, and **exposed the read/write of internal states (commanded motor current, encoder position/velocity, pot position, etc) to the ROS interface** by modifying the ROS bridge in `dvrk-ros`. 

I created a python script (can be found in `dvrk_ros_pmd`) to **serve as a bridge between the aforementioned states in `sawRobotIO1394` and the states on the PMD hardware**. It communicates with PMD hardware via UDP packets over ethernet. It communicates with `sawRobotIO1394` through ROS.

`pmd-davinci-firmware` runs on PMD. It parses the UDP packets from `dvrk_ros_pmd` and writes the desired motor output to the Atlas motor amplifiers. It reads encoder/pot/motor current and sends them as a broadcast packet over UDP to `dvrk_ros_pmd`.

`pypmd` talks to PMD via the Remote Access Protocol (based on TCP packets). It is a python re-implementation of the PMD host api, because the api supplied by PMD heavily depends on windows socket, which does not work on gnu/linux. `pypmd` is good for sending few commands to PMD, but is too slow for real-time control.

# repositories
* `cisst-ros` fork: modified `sawRobotIO1394` and other supporting components https://github.com/urill/cisst-ros
* `dvrk-ros` fork: exposed some internal states to ROS https://github.com/urill/dvrk-ros
* `dvrk_ros_pmd`: python script that runs on your computer, talks to PMD over ethernet, and talks to `sawRobotIO1394` through ROS https://github.com/urill/dvrk_ros_pmd
* `pmd-davinci-firmware`: program that runs on the microcontroller on the PMD https://github.com/urill/pmd-davinci-firmware
* `pypmd`: native python api for PMD Remote Access Protocol https://github.com/urill/pypmd
* `pmd_davinci_adapter`: design files for the pmd -> dvrk interface adapter board https://github.com/urill/pmd_davinci_adapter
* `pmd-dvrk-docker`: *obsolete+broken*. dockerfile to create an environment for this project https://github.com/urill/pmd-dvrk-docker

# hardware setup
![](hw.jpg)

The controller box is pre-wired and ready to use as of Feb 2018.

**The box contains 120V circuit and you should handle it with respect. Unplug the power cable before you modify the circuit. Verify the wiring before you power it back on.**

The left PMD connects to 12V power and joints 5-7 (1-indexed). The right PMD connects to 24V power and joints 1-4.

Make sure the SCSI and DB9 cables are connected between the PMD and the interface board at the rear of the box. 

Each PMD connects to a port on the ethernet switch. They expect a gateway on `192.168.1.1` with the subnet mask of `24`. The static ip address for the left PMD is `192.168.1.42`, and `192.168.1.41` for the right one. If you don't have a router, set the IP address of your computer's ethernet interface to `192.168.1.1`. These tcp/ip parameters can be reconfigured in the Pro-Motion software.

# software setup


# things that don't work
- E-stop not implemented
-

# future work
