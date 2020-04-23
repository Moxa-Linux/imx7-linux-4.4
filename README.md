# Moxa Kernel Repository Guidelines

The following material guides user how to build out kernel image and put the result into the moxa device.

* [How to build the kernel package](#how-to-build-the-kernel-package)
* [How to update kernel on the device](#how-to-update-kernel-on-the-device)
* [Products List](#products-list)
* [Appendix](#appendix)

---
## How to build the kernel package

>>>
Following example is performed on Stretch.
>>>

### Prepare Docker Image
[https://github.com/Moxa-Linux/moxa-dockerfiles](https://github.com/Moxa-Linux/moxa-dockerfiles)

### Create a container

```
# docker run -d -v /tmp/kernel:/tmp/kernel -it moxa_docker:v1 bash
```

* The docker image `moxa_docker:v1` is created in [Prepare Docker Image](#prepare-docker-image).
* `/tmp/kernel/` is a volume used for the access to the kernel source in the docker container.

### Build the kernel package
Enter the container and navigate to `/tmp/kernel/imx7-linux-4.4/` and execute

```
# docker start -ia <docker_container_id>
# cd /tmp/kernel
# dpkg-buildpackage -us -uc -b -aarmhf
```

* `<docker_container_id>` comes from the command `docker run`
* The result should be under `/tmp/kernel/imx7-linux-4.4/../`

---
## How to update kernel on the device

### 1. Upload kernel packages to the device
```
# scp <files> <Username of device system>@<IP address of device>:<File path>
```

Example:
```
# scp uc8200-kernel*.deb uc8200-modules*.deb moxa@192.168.3.100:/tmp
```

### 2. Install the packages

Example:
```
# cd /tmp
# dpkg -i *.deb
# sync
```

### 3. Reboot the device
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
