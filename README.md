# dwarf_bench

“Dwarf bench” is a collection of patterns that attempt to capture performance characteristics of analytical queries. The idea is to extend the taxonomy of computational patterns defined in the article “The Landscape of Parallel Computing Research” published in 2006 to data analytics in heterogeneous environments. Implementing basic structures and algorithms once for multiple devices strives to find a balance between performance and specific capabilities usage, and implementation effort. We chose platform-agnostic tools to express our language of patterns (OpenCL, SYCL).
You can use dwarf_bench on BareMetal/Virtual Machine (see ## Build) or a container (see ## Docker) below

## Build
0. Requirements (see below): boost1.61, oclhelpers, opencl 1.2 (tested with nvidia 11.3, intel gfx & intel opencl cpu runtimes)
1. Get latest release of opencl helpers from https://github.com/kurapov-peter/oclhelpers/releases
2. Put the lib into your CMAKE_PREFIX_PATH or set oclhelpers_DIR env var
3. Install [CUDA](https://developer.nvidia.com/cuda-downloads?target_os=Linux)
4. Install [CPU runtime](https://software.intel.com/content/www/us/en/develop/documentation/installation-guide-for-intel-oneapi-toolkits-linux/top/installation/install-using-package-managers/apt.html) (You'll only need the runtime: `sudo apt install intel-oneapi-runtime-opencl`)
5. Install [Intel gfx drivers](https://dgpu-docs.intel.com/installation-guides/ubuntu/ubuntu-focal.html)
6. In order to run dpcpp tests with cpu, gpu and cuda follow these steps:
    - [Build dpcpp compiler with cuda support](https://intel.github.io/llvm-docs/GetStartedGuide.html#build-dpc-toolchain-with-support-for-nvidia-cuda)
    - Install [onedpl](https://github.com/oneapi-src/oneDPL/) (i.e. along with the [basekit](https://software.intel.com/content/www/us/en/develop/tools/oneapi/base-toolkit.html#gs.24lvfe))
7. mkdir build && cd build && CXX=clang++ oclhelpers_DIR=/path/to/helpers cmake -DENABLE_DPCPP=ON .. && make -j`nproc`

## Docker
* docker build . --network host -t dwarfs-dev  
* docker run --network host --privileged -it --name spicy -v /path/to/dwarf_bench:/dwarf_bench dwarfs-dev:latest bash
* mkdir build && cd build
* CXX=dpcpp cmake /dwarf_bench/ -DENABLE_DPCPP=on -DENABLE_TESTS=on
* cmake --build . --parallel 4
* cd tests && ctest
