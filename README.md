# pmd dvrk project
**A compatible dvrk controller based on PMD Prodigy/CME machine controller.** This repository only contains documentation and links to the components.

# how it works
The PMD machine controller is a complete motor control solution built with an ARM microcontroller to run user code, an FPGA to handle IO, and motor amplifiers (Atlas). The Atlas has a microcontroller for software current control and three half bridges. Because the current control runs in software (instead of hardware analog circuit with low-pass filter in QLA), you get higher bandwidth and tuneable current control performance.

I wanted to make this PMD controller a drop-in replacement for the QLA+firewire controller. Unfortunately, the cisst-saw kit was not written with alternative motor controller in mind. 


# repositories
* `cisst-ros` fork: modified `sawRobotIO1394` and supporting components https://github.com/urill/cisst-ros
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
