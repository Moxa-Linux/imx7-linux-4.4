# Building Moxa Linux Kernel imx7-linux-4.4 project

Below you will find instructions to build and install the imx7-linux-4.4 project.

## Build Instructions Table

| Branch / Tags | Build Instructions |
| - | - |
| UC-8200_V1.0, UC-8200_V1.1, UC8200_V1.2 <br> 4.4.176-cip31-rt23/stretch/master | Below Instructions (`Latest`) |

## Download source

To obtain the imx7-linux-4.4 sources you must clone them as below:

```
git clone https://github.com/Moxa-Linux/imx7-linux-4.4
```

## Dependencies

To build imx7-linux-4.4, we provide [moxa-dockerfiles](https://github.com/Moxa-Linux/moxa-dockerfiles) to create build environment.

## Building

### Create docker container

To create a docker container execute the following commands from the directory which source in:

```
# sudo docker run -d -it -v ${PWD}:/workspace moxa-package-builder:1.0.0 bash
d103e6df5f719f9430056f9c23cf4e3e518d4a4f8b5b65e55889b90c258886c6
```

After execute commands, you will get a string like `d103e6df5f719f9430056f9c23cf4e3e518d4a4f8b5b65e55889b90c258886c6` which called `<container_id>` and we will use it in next step.

### Build kernel package

To build kernel package execute the following commands:

```
# docker start -ia <container_id>
# cd /workspace/imx7-linux-4.4
# apt-get build-dep -aarmhf .
# dpkg-buildpackage -us -uc -b -aarmhf
```

Once build process complete, you can find `.deb` files under `/workspace` directory.

## Updating

After build the kernel packages, now you can update your device.

Below are instructions to update the kernel packages on `UC-8200`.

### Upload the kernel packages to the device

To upload kernel package to the device execute the following commands:

```
# scp uc8200-kernel*.deb uc8200-modules*.deb moxa@192.168.3.127:/tmp
```

### Install the kernel packages

To install kernel package to the device execute the following commands:

```
# cd /tmp
# dpkg -i *.deb
# sync
```

**NOTE: Remember to reboot the device after install the kernel package!**

## Appendix

### Customize kernel configuration

To configure your own kernel config based on moxa's configuration execute the following commands:

```
# export ARCH=arm
# make imx7d_defconfig
# make menuconfig
```
