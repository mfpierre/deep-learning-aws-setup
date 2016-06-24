# deep-learning-aws-setup

This tutorial was tested using a g2.2x instance on AWS with ubuntu a fresh 14.04 install.

Heavily inspired from https://github.com/saiprashanths/dl-setup but with all the non-AWS steps removed

## Basic tooling

```bash
sudo apt-get update  
sudo apt-get upgrade  
sudo apt-get install build-essential cmake g++ gfortran git pkg-config python-dev software-properties-common wget htop
```

## Nvidia Drivers

Installation

```bash
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-get update
sudo apt-get install nvidia-364
sudo shutdown -r now
```

Check the driver is correctly installed

```
cat /proc/driver/nvidia/version
```
> NVRM version: NVIDIA UNIX x86_64 Kernel Module  364.19  Tue Apr 19 14:44:55 PDT 2016

> GCC version:  gcc version 4.8.4 (Ubuntu 4.8.4-2ubuntu1~14.04.3)

## Install CUDA 7.5
```
wget http://developer.download.nvidia.com/compute/cuda/7.5/Prod/local_installers/cuda_7.5.18_linux.run
sudo sh cuda_7.5.18_linux.run
```
:warning: When asked don't install the driver bundled with CUDA (it's an older version), make sure to install the examples also to check later if everything is alright
