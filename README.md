This kernel source code is copied from https://github.com/MoKee/android_kernel_xiaomi_leo

Only tested on Mokee 8 (LineageOS 15) (Android 8)

If you want to use this kernel, but you don't want to complie it, you can download boot.img at Release page.

Changes:

1. Changed some c(cxx)flags (See at how to build)
2. Added MDSS_kcal support and K-lapse
3. Added new TCP congestion tcp_sociopath algorithm

How to build:

NOTICE: For compile successfully,I added -w in c(cxx)flags and cancelled every warning in scripts/Makefile.build.
	And for making the kernel works faster,I replaced "-O2" with "-Ofast" in Makefile.
	Also, for compling faster (If you just compile for one time,do this will be slower!),I forced enable ccache in Makefile by replacing "gcc(g++)" with "ccache gcc(g++)", if you just compile for one time, change it back, or you should install ccache.
	Because this code is too old, it only provides .h for gcc3-5, as far as I know,only Ubuntu xenial(Ubuntu 16) provides gcc4-5 in their apt respositories.

Ok,now let's compile the kernel!

1. Download this code
2. Install fastboot,ccache,gcc,g++,make,bc,abootimg, if your system use 'dpkg',you can install them easily with 'apt' (ATTENTION: Because I compile the code on a arm64(aarch64) device, I needn't care about the target architecture of gcc(g++), if you are using devices of other architectures, make sure your gcc(g++)'s target architecture is arm64 or aarch64 (PS: A gcc(g++) from NDK is usable too)
3. Get into this code's root directory with your terminal and then $cd output
4. $make -j8 O=output

Then you will get Image.gz-dtb at code_root/output/arch/arm64/boot/

How to flash the kernel:

1. Copy Image.gz-dtb to a directory that you like
2. Copy boot.img from your phone to that directory, too
3. Get into that directory with your terminal, then $abootimg -u boot.img -k Image.gz-dtb
4. Reboot your phone to bootloader mode, then $fastboot flash boot boot.img, then $fastboot reboot

Now your phone is working with this kernel!

If you want to change some make options:

Run $make menuconfig or $make xconfig at [code_root]/output/
This will edit [code_root]/output/.config
ATTENTION: menuconfig depends on ncurses-dev, xconfig depends on libqt4-dev and pkg-config, you should install them before you run these commands
