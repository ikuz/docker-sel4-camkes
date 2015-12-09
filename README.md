# Docker image for seL4 and CAmkES build tools

This repository contains a Docker image for building and running
[seL4](http://seL4.systems) and [CAmkES](http://seL4.systems/CAmkES) locally.

## Bootstrapping container

First of all, the image has to be built or fetched from
[hub.docker.com](https://registry.hub.docker.com/u/ikuz/sel4-camkes-build/)
with `make build` or `make pull` respectively.

Next, a container has to be created from the image.
`make create` will do the job. With this container you can download the source 
code into the container and build it there. 

Alternatively `make create_local SRC_PATH=...` creates a container that uses 
locally available source and builds in a local directory. `SRC_PATH`
corresponds to a seL4/CAmkES source tree on the local machine. This will be 
available under `/build_src` in the container.

Finally, `make start` will open a terminal from which seL4/CAmkES can be compiled
and run.

## Compiling and running

In order to compile the OS for the first time, the following commands need to
be executed after entering the terminal.

If you used `make create`:

	cd /home/root
	mkdir camkes-manifest
	cd camkes-manifest
	repo init -u 
	repo sync

Otherwise if you used `make create_local`

	cd /build_src

To build for ARM

	make arm_simple_defconfig
	make silentoldconfig
	make

Afterwards, launching the OS via `qemu` can be done by

	qemu-system-arm -M kzm -nographic -kernel images/capdl-loader-experimental-image-arm-imx31

To build for x86

	make x86_simple_defconfig
	make silentoldconfig
	make

Afterwards, launching the OS via `qemu` can be done by

	qemu-system-i386 -nographic -m 512 -kernel images/kernel-ia32-pc99 -initrd images/capdl-loader-experimental-image-ia32-pc99	

For further details please refer to [the seL4 website](http://sel4.systems).


