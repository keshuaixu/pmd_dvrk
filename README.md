# pmd dvrk project
**A compatible dvrk controller based on PMD Prodigy/CME motor driver.** This repository only contains documentation and links to the components.

# repositories
* `cisst-ros` fork: modified `sawRobotIO1394` and supporting components https://github.com/urill/cisst-ros
* `pmd-davinci-firmware`: program that runs on the microcontroller on the PMD https://github.com/urill/pmd-davinci-firmware
* `pypmd`: native python api for PMD Remote Access Protocol https://github.com/urill/pypmd
* `pmd_davinci_adapter`: design files for the pmd -> dvrk interface adapter board https://github.com/urill/pmd_davinci_adapter
* `pmd-dvrk-docker`: *obsolete*. dockerfile to create an environment for this project https://github.com/urill/pmd-dvrk-docker

# hardware setup
![](hw.jpg)

The two PMD connects to a ethernet switch. They expect a gateway on `192.168.1.1`. The subnet mask is `24`. The address for the left PMD is `192.168.1.42`, and `192.168.1.41` for the right one.



# things that don't work
* E-stop not implemented
* 
