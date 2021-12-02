## Flowstat for VPP 20.01

## mandatory patch for VPP
0001 fixes memory leak in VPP source code. pull request was sent to FDio/vpp and till it is accepted use this patch.
## convenient patches for VPP (they are independent of each other)
9001 adds display of offending PC to signal handler for aarch64.

9002 adds display of registers to signal handler for aarch64.

9003 adds -funwind-tables to CMakeLists.txt

9004 add run-time warning of incorrect process stack alignment

9005 shows how to correct process stack alignment (most likely removes warnings like: process stack: Invalid argument (errno 22))

## Build VPP with flowstat
get VPP
```
git clone https://github.com/FDio/vpp.git
cd vpp
git checkout tags/v20.01
```
get DPI
```
cd ..
git clone https://gitlab.cubro.org/dpi-devs/vpp-dpi.git
cd vpp-dpi
git checkout -b vpp_v20.01_flowstat remotes/origin/vpp_v20.01_flowstat
```
copy DPI sources
```
cd ../vpp
cp -r ../vpp-dpi/src/plugins/dpi src/plugins/
```
apply mandatory patch
```
git apply src/plugins/dpi/0001-vnet-Memory-leak-unreleased-frame.patch
```
apply convenient patches shall need arise
```
git apply src/plugins/dpi/9001-vppinfra-signal-handling-decode-pc.patch
git apply src/plugins/dpi/9002-vlib-signal-handling-decode-registers.patch
git apply src/plugins/dpi/9003-correct-cmake.patch
git apply src/plugins/dpi/9004-vlib-warn-of-incorrect-stack-alignment.patch
git apply src/plugins/dpi/9005-vlib-correct-stack-alignment.patch
```
make
```
make build
```
## The following part is out dated and might or might not work
Clone VPP (version 18.10)

```
git clone https://github.com/FDio/vpp.git
cd vpp
git checkout -b v18.10 origin/stable/1810
cd ..
```

For ARM64 builds using multiarch, first register qemu
```
docker run --rm --privileged multiarch/qemu-user-static:register --reset
```

If you are running VPP in container, build the image first
```
docker build -t vpp-dpi:x86_vpp_v1810 .
```
or for arm64
```
docker build --no-cache -t vpp-dpi:arm64_vpp_v1810 -f arm64.Dockerfile .
```

Run the image
```
docker run -it --privileged -v $(pwd):/root/vpp vpp-dpi:x86_vpp_v1810
```
or for arm64
```
docker run -it --privileged -v $(pwd):/root/vpp vpp-dpi:arm64_vpp_v1810
```

Build VPP depenedencies
```
cd vpp/
yes | make install-dep
make install-ext-deps
```

Copy plugins dpi and crypto_native into `src/plugins/dpi` directory and build VPP
```
cp -r ../src/plugins/* src/plugins/
make build
```

optionally make links to executables vpp and vppctl
```
ln -fs /root/vpp/vpp/build-root/install-vpp_debug-native/vpp/bin/vppctl /usr/bin/vppctl
ln -fs /root/vpp/vpp/build-root/install-vpp_debug-native/vpp/bin/vpp /usr/bin/vpp
```

Setup VPP, an example of startup.conf (and included dpi.conf) is in source code.

## Run VPP
```
vpp –c ../startup.conf
```
