# DPDK Training

I will try to cover DPDK from basic to advance with example code.
And also explain about DPDK internal to understand it in detail.

To gain more, you must practice example by running them.

I am using ubuntu 20.04 OS and current DPDK stable release version v20.11.

## Installation

* Execute below command to setup development environment.
```
apt install build-essential
apt install python3 python3-pip
pip3 install meson ninja
apt install python3-pyelftools
apt install libnuma-dev
apt install wget git vim
apt install -y pkg-config
```

Please refer to [DPDK document](https://doc.dpdk.org/guides/linux_gsg/sys_reqs.html#compilation-of-the-dpdk)
to install packages as per your selected linux version / other OS.

* Checkout DPDK source code.
```
git clone --branch v20.11 https://github.com/DPDK/dpdk.git dpdk_v20
```

* Compile code using below command
```
meson build
cd build
ninja
ninja install
pkg-config --cflags libdpdk
pkg-config --libs libdpdk
```

## DPDK

Each tab explain about perticular module/library within DPDK along with example.

* [EAL](modules/eal.md)
* [Threading Module](modules/threading.md)
* [Memory Pool](modules/mempool.md)
* [Packet Pool](modules/packet_buffer.md)
