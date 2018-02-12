My notes to get the nvidia graphics card working on sabayon, a Gentoo based
Linux distro. 

## Installation
* The kernel version of the driver should match the kernel version on the OS.
* Install the drivers from the repos. Current version is 387.34. Package names
  are nvidia-drivers, nvidia-userspace-drivers. 
* Install bumblebee
* Check the number for nvidia on `eselect opengl list`. Set it to use nvidia by
  `eselect opengl set 1` (in this case, nvidia is 1). 


## Known issues and work arounds
* This crashes my xserver on start up. So, drop to a tty and load the kernel
  module (`sudo modprobe nvidia`) `startx -- :1` there. After every restart.
  Still needs a fix. 
* Without the `eselect` step, running `steam` causes error "OpenGL GLX
  extension not supported by display".  
* Without the `eselect` step, running `optirun steam` causes error
  "glXChooseVisual failed". Both errors are fixed with the command. Needs to
  run only once. 
* After running `eselect`, running `steam` causes error "OpenGL GLX extension
  not supported by display". Fixed by running `optirun steam`.
* Steam shows a "Connection Error - Could not connect the steam cloud". Keep
  retrying and it works exactly on the fourth time. 
* Some games don't start, some show the splash screen and disappear, some work
  and crash unexpectedly. Fixed by setting launch options in steam to the
  following:
    ```shell
    LD_PRELOAD="${LD_PRELOAD/libdlfaker.so:libvglfaker.so:/}:libdlfaker.so:libvglfaker.so" %command%
    ```
* CPU temps rise, and seem to settle at 72-74 degrees C, when playing steam
  games. My system idles at 42-44 deg C.

## My hardware
* Acer Aspire E 15, E5-575G-53VG
* Processor: Intel Core i5-6200U 2.3GHz
* Graphics: Nvidia GeForce 940MX with 2GB dedicated VRAM (DDR5)
* Memory: 8GB DDR4
* Storage: 256GB SSD

## OS Details
* screenfetch
```shell
	$screenfetch 
		    ...........                bk@BlitzKomp
		 ..             ..             OS: Sabayon 
	      ..                   ..          Kernel: x86_64 Linux 4.14.0-sabayon
	    ..           :           ..        Uptime: 1h 44m
	  ..            .:.            ..      Packages: 1866
	 ..             .:.             ..     Shell: bash 4.4.12
	:.             .:::.             .:    Resolution: 1920x1080
	:.             .:::.             .:    WM: bspwm
	:              .:::.          :   :    CPU: Intel Core i5-6200U @ 4x 2.8GHz [45.0°C]
	:   :          .:::.        ...   :    GPU: GeForce 940MX
	:   ....       .:::.    ......    :    RAM: 2306MiB / 7840MiB
	:     .....................       :   
	:.     ...................       .:   
	 ..         ..........           ..   
	   ..         ......          ..      
	     ..                     ..        
		..               ..           
		  ...............             
```

* $uname -a
```
Linux BlitzKomp 4.14.0-sabayon #1 SMP Sat Feb 3 14:07:01 UTC 2018 x86_64 Intel(R) Core(TM) i5-6200U CPU @ 2.30GHz GenuineIntel GNU/Linux
```

## Things that DID NOT work:
* Setting nomodeset in the grub boot options. Would boot, but will not start
  xserver. Can drop to tty, but the resolution is 80x25 characters. So it seems
  it's messing the video drivers.
* `nouveau` drivers

