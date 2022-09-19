# How to Boot into a Specific Kernel Version - Ubuntu and Debian Platforms

Search the `apt` repositories for available kernels.

`sudo apt-get update`

First step is to identify the necessary packages that you need to install. This is done by executing the following command:

`apt-cache search --names-only linux-image`


Eg: `apt-cache search --names-only linux-image | grep "5.4.0-122-generic"`

This returns a list of available kernels similar to this:

```
linux-image-5.4.0-122-generic - Linux kernel image for version 5.4.0 on 64 bit x86 SMP
linux-image-5.4.0-122-lowlatency - Linux kernel image for version 5.4.0 on 64 bit x86 SMP
linux-image-extra-5.4.0-122-generic - Linux kernel extra modules for version 5.4.0 on 64 bit x86 SMP
. . .
```

The package name of the kernel is `linux-image-` followed by the version, like `linux-image-5.4.0-122-generic`. The associated headers file for a kernel has the same package name with `image` replaced by `headers`.

Afterwards you know which package needs to be installed:

`sudo apt-get install linux-image-5.4.0-122-generic linux-headers-5.4.0-122-generic`

below command will show you the list of installed images:

`dpkg --list | grep linux-image`

In the next step the actual kernel gets removed:

`sudo apt remove linux-image-5.4.0-125-generic`

if anyother image unsigned remove that aswell:

`sudo apt remove linux-image-unsigned-5.4.0-125-generic`

During the process you confirm with `No` that you do not want to abort the removal process:

Kernel removal confirmation window

As the last step you initiate a reboot with

`sudo reboot`