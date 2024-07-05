# vSoC Build Instructions

vSoC is a virtual mobile SoC and therefore consists of two parts, the guest system and the host emulator. This document details the procedures required to build the two components.

Note: this document is still work-in-progress!

## Guest Image (Android)

### Prerequisites

A 64-bit x86 linux machine whose **minimal** hardware configuration includes:
 * 4-core CPU
 * 16 GB memory
 * 300 GB free disk storage
 * a working Internet connection

Machines with fast CPUs, SSDs, and ≥100Mbps connections are recommended. 

### Build Steps

1. Firstly, refer to the AOSP page ["Establishing a Build Environment"](http://source.android.com/source/initializing.html) to configure your build environment. For Ubuntu 22.04, install the following required packages:

```bash
sudo apt -y install repo libxml2-utils flex m4 openjdk-8-jdk lib32stdc++6 libelf-dev mtools libssl-dev python3-mako syslinux-utils
```

2. Pull down vSoC Android guest source to your working directory with the following commands. Note the projects created or modified by us are fetched from our github organization. All the other projects are still downloaded from AOSP. If you experience network errors with `repo sync`, please re-run the command until sync finished successfully.

```bash
mkdir vsoc && cd vsoc
repo init -u https://github.com/VirtualSoC/android_manifest -b vsoc
repo sync
```

3. Setup the Android build system with the following commands.

```bash
source build/envsetup.sh
lunch android_x86_64-userdebug
```

4. Start building. Building Android from scratch can take a few hours to complete, so please wait patiently. If you encountered an error, please [file an issue](https://github.com/VirtualSoC/VirtualSoC.github.io/issues/new/choose) or contact us.

```bash
m -j$(nproc) iso_img
```

5. When build completes, you can collect the resulting vSoC guest image at `./out/target/product/x86_64/android_x86_64.iso`.

## Guest Image (OpenHarmony)

TBD

## Host Emulator (Windows)

### Prerequisites

A Windows-x64 machine whose **minimal** hardware configuration includes:
 * 4-core CPU
 * 8 GB memory
 * 16 GB free disk storage

### Build Steps

1. Download and install MSYS2 following the instructions at [https://www.msys2.org/](https://www.msys2.org/). Do **NOT** install MSYS2 in paths that contain spaces, e.g., C:/Program Files.

2. Open the MSYS2 MinGW x64 terminal (**NOT** MSYS2 MSYS terminal) and install dependencies using:

```bash
pacman -S base-devel git mingw-w64-x86_64-binutils mingw-w64-x86_64-crt-git mingw-w64-x86_64-headers-git mingw-w64-x86_64-gcc-libs mingw-w64-x86_64-gcc mingw-w64-x86_64-gdb mingw-w64-x86_64-make mingw-w64-x86_64-tools-git mingw-w64-x86_64-pkg-config mingw-w64-x86_64-winpthreads-git mingw-w64-x86_64-libwinpthread-git mingw-w64-x86_64-winstorecompat-git mingw-w64-x86_64-libmangle-git mingw-w64-x86_64-pixman mingw-w64-x86_64-SDL2 mingw-w64-x86_64-glib2 mingw-w64-x86_64-capstone mingw-w64-x86_64-glfw mingw-w64-x86_64-lzo2 mingw-w64-x86_64-libxml2 mingw-w64-x86_64-libjpeg-turbo mingw-w64-x86_64-libpng mingw-w64-x86_64-ffmpeg
cp /mingw64/bin/ar.exe /mingw64/bin/x86_64-w64-mingw32-ar.exe & cp /mingw64/bin/ranlib.exe /mingw64/bin/x86_64-w64-mingw32-ranlib.exe & cp /mingw64/bin/windres.exe /mingw64/bin/x86_64-w64-mingw32-windres.exe & cp /mingw64/bin/objcopy.exe /mingw64/bin/x86_64-w64-mingw32-objcopy.exe  & cp /mingw64/bin/nm.exe /mingw64/bin/x86_64-w64-mingw32-nm.exe & cp /mingw64/bin/strip.exe /mingw64/bin/x86_64-w64-mingw32-strip.exe
```

3. Pull down the source code of vSoC host emulator with:

```bash
git clone https://github.com/VirtualSoC/qemu.git
cd qemu
git submodule update --init --recursive
```

4. Configure and build the emulator. It typically takes ~15 minutes to complete.

```bash
./configure --cross-prefix=x86_64-w64-mingw32- --disable-gtk --enable-sdl --enable-whpx --target-list=x86_64-softmmu --disable-werror
make install -j$(nproc)
```

## Host Emulator (macOS)

TBD
