# DPDK Training

In this we will cover DPDK basic with example code and try to understand how it
work. Also will try to cover detail about DPDK internal.

To gain, you must practice example by running them.

In this, we will use ubuntu 20.04 OS and current DPDK release version.

## Installation

* Execute below command to setup development environment.
```
apt install build-essential
apt install python3 python3-pip
pip3 install meson ninja
apt install python3-pyelftools
apt install libnuma-dev
apt install wget git vim
```

Please refer to [DPDK document](https://doc.dpdk.org/guides/linux_gsg/sys_reqs.html#compilation-of-the-dpdk)
to install packages as per your selected linux version.

* Checkout DPDK source code.
```
git clone --branch v20.11 https://github.com/DPDK/dpdk.git dpdk_v20
```

* Compile code using below command
```
meson build
ninja
ninja install
```

## Example

Each example in section will explain about DPDK module/library in detail.

* [Argument Parsing](modules/args.md)
* [Threading Module](modules/threading.md)
