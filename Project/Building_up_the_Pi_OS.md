### Poky has no support for Broadcom BCM SoC. Download the BSP Layer for Raspberry Pi3. Download meta-openembedded branch as meta-raspberrypi layer depends upon it. Clone the mata-raspberrypi and meta-openembedded git repositories using git command in the same directory of poky.
```bash
git clone git://git.yoctoproject.org/meta-raspberrypi
git clone git://git.openembedded.org/meta-openembedded
```
#### Once you complete cloning, there should be three folders: poky, meta-openembedded, meta-raspberrypi
```bash
ls
```

### All the three folder layers should have the same branch i.e. the ```master``` branch.
```bash
cd poky
git checkout master
cd ..
cd meta-raspberrypi
git checkout master
cd ..
cd meta-openembedded
git checkout master
cd ..
```

### Run the environment script to setup the Yocto Environment and create build directory for RaspberryPi-3
```bash
cd poky
source oe-init-build-env build_raspi
```
### By default there are only three-layers present in the bblayers.conf file. 
### But for RaspberryPi-3 we have to add more layers from the meta-openembedded layers and meta-raspberrypi layer to bblayers.conf file.
### From meta-openembedded we have to include: 
* meta-oe layer
* meta-multimedia layer
* meta-networking layer
* meta-python layer
```bash
bitbake-layers add-layer ../../meta-openembedded/meta-oe/
bitbake-layers add-layer ../../meta-openembedded/meta-multimedia/
bitbake-layers add-layer ../../meta-openembedded/meta-networking/
bitbake-layers add-layer ../../meta-openembedded/meta-python/
```
### Now let's add the meta-raspberrypi layer to bblayers.conf file.
```bash
bitbake-layers add-layer ../../meta-raspberrypi/
```
### Now we can check the layers in the bblayers.conf file by putting up this command.
```bash
bitbake-layers show-layers
```
### Expected output:
```
NOTE: Starting bitbake server...
layer                 path                                                                    priority
========================================================================================================
core                  /home/amrit/poky/meta                                                   5
yocto                 /home/amrit/poky/meta-poky                                              5
yoctobsp              /home/amrit/poky/meta-yocto-bsp                                         5
raspberrypi           /home/amrit/meta-raspberrypi                                            9
openembedded-layer    /home/amrit/meta-openembedded/meta-oe                                   5
meta-python           /home/amrit/meta-openembedded/meta-python                               5
multimedia-layer      /home/amrit/meta-openembedded/meta-multimedia                           5
networking-layer      /home/amrit/meta-openembedded/meta-networking                           5
```
### Set the MACHINE in local.conf to "raspberrypi3".
```
echo 'MACHINE = "raspberrypi3"' >> conf/local.conf
```
### linux-firmware-rpidistro
#### By default, some of the machine configurations recommend packages for the WiFi/BT firmware, provided by linux-firmware-rpidistro. This package includes some firmware blobs under the ```Synaptics``` license which could carry a legal risk: one of the clauses can be (at least theoretically) used as a ```killswitch```. This was reported in the upstream repository.
#### Due to this, the build system will only allow this recipe to be built if the user acknowledges this risk by adding the following configuration:
* ```LICENSE_FLAGS_ACCEPTED = "synaptics-killswitch"```
#### You can provide this configuration as part of your ```local.conf```
### To enable ```UART``` you can provide this configuration as part of your ```local.conf```
* ```ENABLE_UART = "1"```
### Summing up the ```local.conf``` file, it should have the following configurations:
```bash
MACHINE = "raspberrypi3"
INHERIT += "rm_work"
ENABLE_UART = "1"
LICENSE_FLAGS_ACCEPTED = "synaptics-killswitch"
```
### Final step is to build the image. To find out the available images:
```
ls ../../meta-raspberrypi/recipes-*/images/
```
### Expected output:
```bash
rpi-test-image.bb
```
### Let's build the RaspberryPi-3 image.
```bash
bitbake rpi-test-image
```
