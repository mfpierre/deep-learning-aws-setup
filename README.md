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

Install the Nvidia driver

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

Add CUDA to the environment variables

```
echo 'export PATH=/usr/local/cuda/bin:$PATH' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
```

You can check CUDA is correctly installed
```
nvcc -V
```

> nvcc: NVIDIA (R) Cuda compiler driver

> Copyright (c) 2005-2015 NVIDIA Corporation

> Built on Tue_Aug_11_14:27:32_CDT_2015

> Cuda compilation tools, release 7.5, V7.5.17

Restart the instance
```
shutdown -r now
```

Compile & check the example
```
cd ~/NVIDIA_CUDA-7.5_Samples/
make -j $(($(nproc) + 1))
bin/x86_64/linux/release/deviceQuery
```

> Detected 1 CUDA Capable device(s)

## Install CuDNN

To be able to download the cuDNN library, you need to register in the Nvidia website at https://developer.nvidia.com/cudnn

Once you have your account you can download the `cuDNN v5 Library for Linux` and SCP it to your aws instance

```
tar xvf cudnn-7.5-linux-x64-v5.0-ga.tgz
cd cuda
sudo cp */*.h /usr/local/cuda/include/
sudo cp */libcudnn* /usr/local/cuda/lib64/
sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```

You can check with `nvidia-smi` that everything is working:
```
Fri Jun 24 17:07:53 2016
+------------------------------------------------------+
| NVIDIA-SMI 364.19     Driver Version: 364.19         |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GRID K520           Off  | 0000:00:03.0     Off |                  N/A |
| N/A   29C    P0     1W / 125W |     11MiB /  4095MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID  Type  Process name                               Usage      |
|=============================================================================|
|  No running processes found                                                 |
+-----------------------------------------------------------------------------+
```
