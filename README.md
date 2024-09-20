# NVIDIA Assemble Snap

This repository contains source code for an auxiliary snap that
assists in assembling and loading NVIDIA drivers.

Installing this snap implies consent with [NVIDIA
license](https://www.nvidia.com/en-gb/drivers/geforce-license/).

When installed together with a compatible kernel snap, an attempt will
be made to assemble, load and setup NVIDIA graphics drivers on an
Ubuntu Core system.

## Quick Start on Ubuntu Core 24
    
    # snapd2.59 is required
    $ snap refresh snapd
    $ snap install nvidia-assemble --channel 24/stable

## Quick Checks

    $ sudo ls -latr /dev/nvidia*
    $ sudo lsmod | grep nvidia

Expectation is that `/dev/nvidia` devices are available on the system,
and that nvidia drivers are loaded into the kernel.

## How to use drivers from your own snap

Once the drivers are available on the system, one can use
[nvidia-core24](https://github.com/hannsofie/nvidia-core24) gpu-2024
content interface provider in your snap to provide userspace libraries
to access NVIDIA graphics card functionality. This content interface
can be used if stock userspace libraries are sufficient for a given
snap, or if one wants to share userspace libraries across multiple
snaps. Alternatively, one can simply stage one's own userspace
dependencies as required.

Samples of GL & CUDA CLI applications will be available from
[graphics-test-tools](https://github.com/canonical/graphics-test-tools) in the near future. This
is an example snap that vendors existing GL binaries, and complies a
CUDA binary from source. The snapcraft.yaml for this sample removes
staged libraries from the snap, such that at runtime all libraries are
used from the nvidia-core24 gpu-2024 content provider.

## Troubleshooting

First is to check that kernel contains nvidia driver components:

    $ sudo ls /lib/modules/*/kernel | grep nvidia

To troubleshoot further collect dmesg output and output from snap
installations:

    $ sudo journalctl -b -k
    $ sudo journalctl -b -u snap.nvidia-assemble.service
