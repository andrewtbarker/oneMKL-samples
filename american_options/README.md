# `American Options` Sample

American Options Pricing is a model that is based on the Monte Carlo method
and widely used in financial services industry.
The original [CUDA* source code](https://github.com/NVIDIA-developer-blog/code-samples/tree/master/posts/american-options) is migrated to SYCL for portability across GPUs from multiple vendors.

## Purpose

The sample shows the usage of different models of oneMKL Random Number Generation
(RNG) in the American Options Pricing model.

## Key Implementation Details

Usage of different models of oneMKL Random Number Generation are demonstrated
in this sample: Host API and Device API.
See about different models in [the following article](https://www.intel.com/content/www/us/en/developer/articles/technical/onemkl-rng-device-api-in-financial-services-risk.html).

### CUDA source code evaluation

This sample is migrated from the NVIDIA CUDA sample using Intel® DPC++ Compatibility Tool.
See the [american options sample](
https://github.com/NVIDIA-developer-blog/code-samples/tree/master/posts/american-options)
in the `NVIDIA-developer-blog/code-samples` GitHub.

## Set Environment Variables

When working with the command-line interface (CLI), you should configure the
oneAPI toolkits using environment variables. Set up your CLI environment by
sourcing the `setvars` script every time you open a new terminal window. This
practice ensures that your compiler, libraries, and tools are ready for development.

## Migrate the `American Options` Code

### Optimizations using RNG Device API

To measure performance with RNG Device API following changes need to be introduced:
1. Macro `USE_DEVICE_API` is to distinguish between implementation for Device API and Host API.
2. Remove memory allocation that used to store random numbers.
3. Introduce Device API calls in `generate_paths_kernel` because random numbers can
   be generated within the SYCL kernel and can be used immediately to generate paths.
4. Do not call RNG Host API as a separate call.

## Build the `American Options` Code for CPU and GPU

> **Note**: If you have not already done so, set up your CLI
> environment by sourcing  the `setvars` script in the root of your oneAPI installation.
>
> Linux*:
> - For system wide installations: `. /opt/intel/oneapi/setvars.sh`
> - For private installations: ` . ~/intel/oneapi/setvars.sh`
> - For non-POSIX shells, like csh, use the following command: `bash -c 'source <install-dir>/setvars.sh ; exec csh'`
>
> For more information on configuring environment variables, see [Use the setvars Script with Linux* or macOS*](https://www.intel.com/content/www/us/en/develop/documentation/oneapi-programming-guide/top/oneapi-development-environment-setup/use-the-setvars-script-with-linux-or-macos.html).

### On Linux*

1. Change to the sample directory.
2. Build the program.
   ```
   $ mkdir build
   $ cd build
   $ cmake .. or ( cmake -D USE_DEVICE_API=1 .. )
   $ make
   ```
>**Note:**
> - By default, no flags are enabled during the build which supports
    Intel® UHD Graphics, Intel® Gen11, Xeon CPU.
> - Enable the `USE_DEVICE_API` flag during build to run using Intel oneMKL.

3. Run the code

   You can run the programs for CPU and GPU. The commands indicate the device target.

      Run on GPU.
      ```
      make run_host_api # or make run_device_api for RNG Device API usage
      ```
      Run on CPU.
      ```
      export ONEAPI_DEVICE_SELECTOR=opencl:cpu
      make run_host_api # or make run_device_api for RNG Device API usage
      unset ONEAPI_DEVICE_SELECTOR
      ```

## Licenses

Code samples are licensed under the MIT license. See
[License.txt](License.txt) for details.

Third party program Licenses can be found here: [third-party-programs.txt](third-party-programs.txt).

## Notices and Disclaimers

© Intel Corporation. Intel, the Intel logo, and other Intel marks are trademarks of Intel Corporation or its subsidiaries. Other names and brands may be claimed as the property of others.
