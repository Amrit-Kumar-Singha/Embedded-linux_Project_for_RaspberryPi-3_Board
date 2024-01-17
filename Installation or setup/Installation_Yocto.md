# Yocto Project Quick Build

1. Install the Ubuntu 22.04.03 LTS from Microsoft Store.
2. Make sure your Build Host meets the following requirements:
   * At least 90 Gbytes of free disk space.
   * At least 8 Gbytes of RAM.
   * Git 1.8.3.1 or greater.
   * tar 1.28 or greater.
   * Python 3.8.0 or greater.
   * gcc 8.0 or greater.
   * GNU make 4.0 or greater.
4. You must install essential host packages on your build host. The following command installs the host packages based on an Ubuntu distribution:
   ```bash
   sudo apt install gawk wget git diffstat unzip texinfo gcc build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev python3-subunit mesa-common-dev zstd liblz4-tool file locales libacl1
   sudo locale-gen en_US.UTF-8
   ```
5. Use Git to Clone Poky
   ```bash
   git clone git://git.yoctoproject.org/poky
   ```
6. Check out the nanbield branch based on the Nanbield release:
   ```bash
   git checkout -t origin/nanbield -b my-nanbield
   ```
7. Note that you can regularly type the following command in the same directory to keep your local files in sync with the release branch:
   ```bash
   git pull
   ```
8. Initialize the Build Environment: From within the poky directory, run the oe-init-build-env environment setup script to define Yocto Project’s build environment on your build host.
   ```bash
   cd poky
   source oe-init-build-env
   ```
9. Start the Build: Continue with the following command to build an OS image for the target, which is core-image-minimal in this example:
   ```bash
   bitbake core-image-minimal
   ```
10. Simulate Your Image Using QEMU: Once this particular image is built, you can start QEMU, which is a Quick EMUlator that ships with the Yocto Project:
    ```bash
    runqemu qemux86-64 nographic
    ```
    As you are using WSL 2 we have to go with the nographic option inorder to evaluate our custom built Linux Distribution.
