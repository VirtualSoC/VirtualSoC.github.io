
![license](https://img.shields.io/badge/Platform-Android-green "Android")
![version](https://img.shields.io/badge/Version-Pie-yellow "Pie")

![license](https://img.shields.io/badge/Platform-OpenHarmony-green "OpenHarmony")
![version](https://img.shields.io/badge/Version-4.0-yellow "4.0")

![license](https://img.shields.io/badge/Hypervisor-QEMU-green "QEMU")
![version](https://img.shields.io/badge/Version-7.1-yellow "Pie")

## Table of Contents
- [Introduction](#introduction)
- [Architecture](#architecture)
- [Open Source Progress](#open-source-progress)


## Introduction

Emerging mobile applications such as ultra-high-definition (UHD) video streaming, AR/VR, and cloud gaming require accessing diverse hardware such as video codecs, cameras, and image processors. However, today's mobile emulators exhibit poor performance when emulating these *high-throughput* hardware devices. 
We pinpoint the major reason to be the discrepancy between
the guest and host's memory architecture for hardware devices, *i.e.*, 
the mobile guest's centralized memory on 
system-on-chip (SoC) versus the PC/server's separated memory modules on individual hardware. Such a discrepancy makes 
the shared virtual memory (SVM) architecture of mobile emulators highly inefficient.

To address this,
we design and implement vSoC, a first-of-its-kind virtual mobile SoC
that enables virtual devices to efficiently share data through a *unified* SVM framework for virtual devices.
We then build upon the SVM framework a novel prefetch engine that 
effectively hides the overhead of coherence maintenance (which guarantees devices sharing
the same virtual memory see the same data), a performance bottleneck in traditional emulators.
We implement vSoC for two mobile OSes (Android and OpenHarmony).
Extensive evaluations show that vSoC achieves significant performance improvements compared to state-of-the-art emulators:
1.7-4.6× frame rates and 35%-62% lower motion-to-photon latency for various multimedia applications.
vSoC is adopted by the emulator of a major commercial mobile IDE.

## Architecture

The architecture of vSoC is as follows.

![vSoC Full Architecture](img/vsoc_arch_full.jpg)

vSoC is a paravirtualized mobile SoC primarily based upon QEMU 7.1, and hosts Android-x86 9.0, as well as OpenHarmony 4.0. As shown in the figure, vSoC involves both host- and guest-side modules and enables virtual devices to collaborate and share data efficiently.

More detailedly, the host-side of vSoC is implemented as a **single** QEMU device, which involves the SVM framework and multiple virtual SoC devices (including GPU, display, ISP, codec, camera and cellular modem) that can be individually turned on or off with command line options.

In the guest, the SVM framework is built into the kernel as a set of kernel modules, and virtual device drivers are built as kernel modules that depend on the framework.
vSoC also contains a set of userspace drivers for a close integration with mobile OS frameworks.

## Open Source Progress

**Update 2024.7.3**: we have obtained permission to release vSoC source code. Since vSoC includes both guest and host-side implementations and involves tens of repositories, we are still organizing our codebase to facilitate building. We are also working on documentation, e.g., [build instructions](build.md).

Currently, we are scrutinizing our codebase for security concerns and following internal procedures for open sourcing code.
We expect the relevant code and data to be released soon.