# Moxa Kernel Repository Guidelines

## Table Of Contents
* Getting Started
* How to update kernel
* Products List
* Appendix

---
## Getting Started

>>>
We suggest to run under Debian distribution.

Following example is performed on Stretch
>>>

### Install required packages
```
apt-get update
apt-get install crossbuild-essential-armhf bc liblz4-tool
```

### Check out the kernel source:

#### If you want to build the latest imx7-linux-4.4 kernel source code. Please make sure you are at develop branch.
```
# git branch -a
```

#### If you want to build the specific version of imx7-linux-4.4 kernel source code. Please checkout the kernel source using tag.

##### To show all the tags.
```
# git tag
```

##### Checkout kernel source by using tag:
```
# git checkout <product>_V<version>
```
### Steps to compile the kernel:

The detail model support list, default kernel configuration, dts file, its file and kernel type information can be found at "Product List" below.


#### 1. Set default config.
Example:
```
# make imx7d_defconfig ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf-
```

#### 2. Compile kernel and create zImage file.
Example:
```
# make -j16 ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf-
```

Our kernel source code will build dtb file automatically. If you want to re-build dtb file by your self.
Example:
```
# make imx7d-moxa-uc-8210.dtb ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf-
```

#### 4. Strip and install kernel modules:
Example:
```
# make INSTALL_MOD_STRIP=1 modules_install INSTALL_MOD_PATH=/tmp ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf-
```

#### 5. Create uImage file or build itb file.

To build itb file, please make sure all the <product> dtb and zImage are exist. (For itb kernel type product.)
Example:
```
# ./its/imx7d-moxa-uc-8200
```

---
## How to update kernel
### Steps to update kernel

#### 1. Upload all your compiled results to the device
```
# scp -r <Username of device system>@<IP address of device>:<File path>
```

To upload kernel modules:
Example:
```
# scp -r /tmp/lib/modules/4.4.0-cip-rt-moxa-imx7d/kernel/ moxa@192.168.3.100:/tmp
# scp /tmp/lib/modules/4.4.0-cip-rt-moxa-imx7d/modules.* moxa@192.168.3.100:/tmp
```

To upload itb files:
```
# scp its/imx7d-moxa-uc-8200.itb moxa@192.168.3.100:/tmp
```

#### 2. Replace the kernel modules on the device with new files

Backup the original kernel modules and update kernel modules with new files

Example:
```
# mv /lib/modules/4.4.0-cip-rt-moxa-imx7d/ /lib/modules/4.4.0-cip-rt-moxa-imx7d/
# mkdir -p /lib/modules/4.4.0-cip-rt-moxa-imx7d/
# cp -arf /tmp/kernel/ /lib/modules/4.4.0-cip-rt-moxa-imx7d/
# cp -arf /tmp/modules.* /lib/modules/4.4.0-cip-rt-moxa-imx7d/
# sync
```

#### 3. Replace the kernel file on the device with new file

The kernel file is placed in operation system storage partition 1.
The storage device name can be checked by `lsblk` command:

![lsblk](https://github.com/Moxa-Linux/resize-image/blob/develop/lsblk.PNG?raw=true)

In this example, root is mounted at "/dev/mmcblk0p2", so the system storage device is "/dev/mmcblk0". Partition 1 is "/dev/mmcblk0p1".

To mount operation system storage partition 1

Example:
```
# mount /dev/mmcblk0p1 /mnt
# cd /mnt
```

Backup the original kernel file and update it with new file

Example:
```
# mv imx7d-moxa-uc-8200.itb imx7d-moxa-uc-8200.itb_bak
# cp /tmp/imx7d-moxa-uc-8200.itb .
# sync
```

#### 3. Reboot the device
```
# reboot
```

---
## Products List
### Product kernel configuration:
There following are the list of product kernel configuration files. defconfig is placed in the arch/arm/configs folder, dts file is placed in arch/arm/boot/dts folder, its file is placed in its folder.

#### Kernel Type: itb file
##### UC-8200 series
* models: UC-8210-T-LX, UC-8210-T-LX-S, UC-8220-T-LX, UC-8220-T-LX-US-S, UC-8220-T-LX-EU-S, UC-8220-T-LX-AP-S
* defconfig: imx7d_defconfig
* dts: imx7d-moxa-uc-8210.dts, imx7d-moxa-uc-8220.dts
* its: imx7d-moxa-uc-8200.its

---
## Appendix

### How to create a customized image?
To create a customized image, please refer to the following link and see "Steps to create a customized image" section.
[https://github.com/Moxa-Linux/resize-image](https://github.com/Moxa-Linux/resize-image)
