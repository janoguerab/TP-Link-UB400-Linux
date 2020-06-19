# TP-Link-UB400-Linux
Instalación de TP-Link UB400 en Kali Linux

Basado en: https://www.reddit.com/r/AnnePro/comments/e76ij8/csr_40_bluetooth_dongle_on_linux/

## Get the source files

You need to get the source code for exact kernel version you are running. Run in your terminal the next command and check out your kernel version.

```
$ uname -r
5.4.2-arch1-1

```
Note that the "-arch1-1" part is the kernel extra version needed later to compile the module.

Make a build folder in your home directory to save the source files in it.

```
$ mkdir ~/build

```
Next, go to [kernel.org](https://www.kernel.org) and download the respective kernel source file (tarball). Also download the patch for the btusb module from this [link](https://github.com/janoguerab/TP-Link-UB400-Linux/blob/master/btusb-Enablement-of-HCI_QUIRK_BROKEN_STORED_LINK_KEY-quirk.patch). Change to the build folder and extract the kernel source:

```
$ cd ~/build
$ tar -xvJf linux-5.4.2.tar.xz 

```
The ls command should show the following:

```
$ ls
btusb-Enablement-of-HCI_QUIRK_BROKEN_STORED_LINK_KEY-quirk.patch
linux-5.4.2
linux-5.4.2.tar.xz

```
## Configure the source files

Change into the new kernel source directory created, and clean the kernel tree:

```
$ cd linux-5.4.2/
$ make mrproper

```
Then you need to copy your current existing kernel configuration to this build dir:

```
$ cp /usr/lib/modules/5.4.2-arch1-1/build/.config ./
$ cp /usr/lib/modules/5.4.2-arch1-1/build/Module.symvers ./

```
Run the next command to apply the copied configuration:

```
$ make oldconfig

```
## Patch the module

Run the following command to patch the btusb module:

```
$ patch -p1 < ../btusb-Enablement-of-HCI_QUIRK_BROKEN_STORED_LINK_KEY-quirk.patch

```
## Compile the module

Prepare the source for module compilation:

```
$ make EXTRAVERSION=-arch1-1 modules_prepare

```

and then compile the bluetooth modules:

```
$ make prepare
$ make scripts
$ make M=drivers/bluetooth modules
```
