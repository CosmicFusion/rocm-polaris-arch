# ROCm Polaris/GFX803 for Arch Linux
This repository hosts a collection of [Arch Linux](https://www.archlinux.org/)
[PKGBUILDs](https://wiki.archlinux.org/index.php/PKGBUILD) for the
[AMD ROCm Platform](https://www.amd.com/en/graphics/servers-solutions-rocm).
With Patched libraries for Polaris/GFX803 .

## Installation
The Arch Linux packages for ROCm Polaris are available on the
[AUR](https://wiki.archlinux.org/index.php/Arch_User_Repository).
Since many packages will be installed, it is recommended to use an
[AUR helper](https://wiki.archlinux.org/index.php/AUR_helpers)
like [`paru`](https://aur.archlinux.org/packages/paru/).

Currently Only 2 Packages need patching to work with Polaris/GFX803, which are the [rocm-opencl-runtime](https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime) and [rocblas](https://github.com/ROCmSoftwarePlatform/rocBLAS) .

And the [rocm-arch](https://github.com/rocm-arch) repository already patched [rocm-opencl-runtime](https://github.com/RadeonOpenCompute/ROCm-OpenCL-Runtime) , leaving rocblas un-patched which mean you will have to get it from here :

You have 2 options : 

Option 1 :

the "bin" package re-packaged from [Xuhuisheng](https://github.com/xuhuisheng) ROCm repo .
Which can be installed from the AUR , by using this command
```bash
paru -S rocblas-polaris-bin
```

Option 2 :

the source package .
Which can be installed from the AUR , by using this command
```bash
paru -S rocblas-polaris
```

## Recommendations for building from source , or using the source AUR Package

ROCm stack comprises around 50 packages including a fork of LLVM.
Therefore, building all packages from source can take a long time and can use a lot of RAM.
If you are experiencing the latter when building `rocm-llvm` set the number of threads for its compilation via the environment variable `NINJAFLAGS`,
```bash
export NINJAFLAGS="-jXX"
```
where `XX` is the number of threads you would like to use.

To speed up compilation of application libraries like `rocblas` or `rocfft` export `AMDGPU_TARGETS`
and set it to the architecture name of your GPU. To find out this name, install `rocminfo`,
```bash
paru -S rocminfo
```
and call
```bash
rocminfo | grep gfx
```
for VEGA 56/64 the output is
```bash
  Name:                    gfx900
        Name:                    amdgcn-amd-amdhsa--gfx900:xnack-
```
Hence, you have to set `AMDGPU_TARGETS` to `gfx900`,
```bash
export AMDGPU_TARGETS="gfx900"
```

For additional installation configuration, such as adding a user to the `video`
group, we refer to AMD's
[installation guide](https://docs.amd.com/bundle/ROCm-Installation-Guide-v5.0.2/page/Prerequisite_Actions.html#d3919e648).
