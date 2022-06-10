# The Linux kernel

The Raspberry Pi kernel is stored in GitHub and can be viewed at github.com/raspberrypi/linux; it follows behind the main Linux kernel. The main Linux kernel is continuously updating; we take long-term releases of the kernel, which are mentioned on the front page, and integrate the changes into the Raspberry Pi kernel. We then create a 'next' branch which contains an unstable port of the kernel; after extensive testing and discussion, we push this to the main branch.

# Building the Kernel

The default compilers and linkers that come with an OS are configured to build executables to run on that OS - they are native tools - but that doesn’t have to be the case. A cross-compiler is configured to build code for a target other than the one running the build process, and using it is called cross-compilation.

## Building the Kernel Locally

On a Raspberry Pi, first install the latest version of Raspberry Pi OS. Then boot your Raspberry Pi, log in, and ensure you’re connected to the internet to give you access to the sources.

First install Git and the build dependencies:

```bash
sudo apt install git bc bison flex libssl-dev make
```
Next get the sources, which will take some time: 
Tips:
If you live in **China** OR somewhere who banned firewall **PLZ USE VPN TOOL**.

```bash
git clone --depth=1 https://github.com/Boos4721/raspi-linux-k5.10
```
### For Raspberry Pi 4 and 400, and Raspberry Pi Compute Module 4 default 32-bit build configuration

```bash
cd linux
KERNEL=kernel7l
make bcm2711_defconfig
```
### For Raspberry Pi 3, 3+, 4, 400 and Zero 2 W, and Raspberry Pi Compute Modules 3, 3+ and 4 default 64-bit build configuration

```bash
cd linux
KERNEL=kernel8
make bcm2711_defconfig
```

### Building the Kernel
Build and install the kernel, modules, and Device Tree blobs; this step can take a long time depending on the Raspberry Pi model in use.
#### For the 32-bit kernel:
```bash
make -j4 zImage modules dtbs
sudo make modules_install
sudo cp arch/arm/boot/dts/*.dtb /boot/
sudo cp arch/arm/boot/dts/overlays/*.dtb* /boot/overlays/
sudo cp arch/arm/boot/dts/overlays/README /boot/overlays/
sudo cp arch/arm/boot/zImage /boot/$KERNEL.img
```
#### For the 64-bit kernel:
```bash
make -j4 Image.gz modules dtbs
sudo make modules_install
sudo cp arch/arm64/boot/dts/broadcom/*.dtb /boot/
sudo cp arch/arm64/boot/dts/overlays/*.dtb* /boot/overlays/
sudo cp arch/arm64/boot/dts/overlays/README /boot/overlays/
sudo cp arch/arm64/boot/Image.gz /boot/$KERNEL.img
```
