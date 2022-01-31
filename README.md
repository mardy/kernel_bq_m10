# Linux kernel for BQ M10 Ubuntu Touch

This is the kernel source tree for the BQ Aquaris M10 HD tablet (codename
`cooler`), imported from the the [GitHub
repository](https://github.com/ubports/kernel_bq_m10) and with some adjustments
to the branch names:

- `xenial`: this is the default branch, which has been forked off from the
  `stable-canonical` branch and contains the latest development changes.
- `stable-canonical`: this is the branch that Canonical released as the last
  stable branch, and it is most likely the same branch which the kernel
  currently (January 2022) running on the M10 is being built from.
- `devel-canonical`: this is an unreleased branch on which Canonical was
  working on; it seems to be adding support to systemd and snaps, but requires
  some careful testing before getting shipped on Ubuntu Touch devices.
- `ubuntu`: this is an import of the homonymous branch from the GitHub repo;
  it's never been deployed to the devices, and it should probably be considered
  stale.

  
### How to build

The project can be built on most Linux distributions and has been verified to
be buildable on Ubuntu 20.04 (Focal Fossa) once the following dependencies are
installed: 

    apt install abootimg build-essential bc

One also needs to install the cross-compiler for the arm64 architecture, which
can be found
[here](https://android.googlesource.com/platform/prebuilts/gcc/linux-x86/aarch64/aarch64-linux-android-4.9/+archive/android-6.0.1_r81.tar.gz).
Once downloaded, the file needs to be unpacked:

    mkdir compiler && cd compiler
    export GCC_DIR=$(pwd)  # we save the current directory as we'll need it later
    tar xzf ~/Downloads/android-6.0.1_r81.tar.gz
    cd -
 
At this point, the kernel can be built with the following commands:

    export KERNEL_CROSS_COMPILE=$GCC_DIR/bin/aarch64-linux-android-
    ./aquaris_m10-build.sh

By default, the artifacts will be generated in `../KERNEL_OBJ/`. In particular,
the file `../KERNEL_OBJ/boot.img` is the kernel image which can be flashed
on the device with

    fastboot flash boot ../KERNEL_OBJ/boot.img

Happy hacking!
