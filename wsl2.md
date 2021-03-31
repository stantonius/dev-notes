# Notes on the setup of WSL2 using GPU

* [This document from Microsoft](https://docs.microsoft.com/en-us/windows/wsl/install-win10) outlines how to set up WSL2 on Windows 10

* [This document from Nvidia](https://docs.nvidia.com/cuda/wsl-user-guide/index.html) outlines the setup of WSL2 to enable CUDA/GPU in the Linux kernel

## Important Installation Points

You need to follow the steps **in order** (go figure) - these are clearly laid out in the NVIDIA post above.
1. Enable the **Windows 10 dev build (version 2xxxx when `winver` is run)** - only available if you're part of the **Windows Insider Programme** (which you enable on your PC)
2. Install wsl2 and ensure version 2 is the default
3. Install a Linux distribution (Ubuntu **18.02**)
4. Follow the NVIDIA setup with very specific commands in second link above
    * Note that you **cannot use Docker desktop** at the moment (Jan 23rd 2021) - you must install Docker for Linux in the kernel and use the CLI to interact with it
    * Although it says this in the walkthrough, you **do not** need to install a GPU driver within the kernel. All you need to ensure is that **`cuda-toolkit`** is installed, and later on **`nvidia-docker2`** is installed. 

**See docker.md for info in setting up WSL2 for DL**

## Config points

Great [Medium post](https://itnext.io/wsl2-tips-limit-cpu-memory-when-using-docker-c022535faf6f) that describes how to manually configure the memory (and other features) for WSL2. The basics are:
* use `wsl --shutdown` to turn off all wsl instances such as docker-desktop **in Windows terminal** (ie. powershell)
* open the config file `notepad "$env:USERPROFILE/.wslconfig"`
* edit `.wslconfig` such as
```
[wsl2]
memory=3GB   # Limits VM memory in WSL 2 up to 3GB
processors=4 # Makes the WSL 2 VM use two virtual processors
```