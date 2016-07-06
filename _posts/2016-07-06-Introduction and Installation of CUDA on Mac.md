---
layout: post_layout
title: Introduction and Installation of CUDA on Mac
time: 2016年07月06日 Wednesday
location: 纽约
pulished: true
excerpt_separator: "## 2. Introduction of CUDA"
---
> ##### *“You can't connect the dots looking forward; you can only connect them looking backwards. So you have to trust that the dots will somehow connect in yourfuture. You have to trust in something -- your gut, destiny, life, karma, whatever. This approach has never let me down, and it has made all the difference in my life.”*     ---Steve Jobs

In this article I will write about parallel programming and CUDA. Also, I will talk about using OpenCV with CUDA which is important for image processing. Hope this article can help you and save your time. Note: This article is for CUDA beginners.

## Overview

- **Configuration**
- **Introduction of CUDA**
- **Installation of CUDA on Mac OS X**
- **OpenCV with CUDA**
- **FAQ**
- **Reference**

## 1. Configuration

I use my Mac Pro as operating platform. The hardware&software environment and softwares I will install are as follows. You can verify that your system is CUDA-capable here [CUDA-supported GPUs](https://developer.nvidia.com/cuda-gpus). If it is an NVIDIA card that is listed on the CUDA-supported GPUs page, your GPU is CUDA-capable.

#### 1.1 Hardware environment
- **MacBook Pro (Retina, 15-inch, Mid 2014)**
- **Processor : 2.5 GHz Intel Core i7**
- **GPU : NVIDIA GeForce GT 750M**

#### 1.2 Software environment

- **OS X Yosemite version 10.10.5**
- **Apple LLVM version 7.0.2 (clang-700.1.81)**
- **Xcode Version 7.2.1 (7C1002)**

#### 1.3 Software installation

- **CUDA 7.5** 
- **OpenCV 3.1.0**

## 2. Introduction of CUDA

At first, we need to think about the question :

> *How does traditional Hardware designer make computers run faster ?*

There are mainly three methods :

- Faster clocks which means we make a computer works harder !
- Do more computation at every clock cycle which means we make a computer more wise.
- Add more processors which means we ask a lot of small computers work simultaneously **(This is parallel programming)**

Now we know what the parallel programming is. 

CUDA is a parallel computing platform and programming model invented by NVIDIA. We use CUDA programming to transfer data from CPU to GPU for parallel computing. 

GPU can handle multiple tasks simultaneously because it is optimized for parallel processing. Also it contains lots of simple compute units. 

CPU consists of some cores, but it is too few compared with GPU. Also CPU is optimized for sequential serial processing.


## 3. Installation of CUDA on Mac OS X

Then I will explain how to install CUDA 7.5 on your computer under OS X.

#### **3.1 Download CUDA Toolkit :** 

NVIDIA CUDA Toolkit is available at [CUDA-downloads](https://developer.nvidia.com/cuda-downloads). You can choose "Full installer" which is a full version. It is called "cuda_7.5.27_mac.dmg ", about 1.0GB.

#### **3.2 Install CUDA Toolkit** 

Install it just like you install a common application. Open the "dmg" files you download, launch the installer, follow the on-screen prompts.

#### **3.3 Set up the Environment variables**

Set up the required environment variables:

> export PATH=/Developer/NVIDIA/CUDA-7.5/bin:\$PATH
> export DYLD_LIBRARY_PATH=/Developer/NVIDIA/CUDA-7.5/lib:$DYLD_LIBRARY_PATH

#### **3.4 Verification**

After the two steps above, you have successfully installed the CUDA driver and CUDA toolkit. You can check it using `nvcc -V` command.

```
jundeMacBook-Pro:~ jun$ nvcc -V
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2015 NVIDIA Corporation
Built on Mon_Apr_11_13:23:40_CDT_2016
Cuda compilation tools, release 7.5, V7.5.26
```

#### **3.5 A simple example**

You can compile and run a simple example using CUDA. The following example is to compute 1^2, 2^2, 3^2, ... , 64^2. CPU allocate this task to 64 threads in GPU and they will work simultaneously.
Using command `nvcc -o square square.cu`  to compile your ".cu" file, then use command `./square` to run it!

```c
#include <stdio.h>
//This is a simple CUDA example! File name: "square.cu" 

__global__ void square(float * d_out, float * d_in) {
	int idx = threadIdx.x;
	float f = d_in[idx];
	d_out[idx] = f*f;
}

int main(int argc, char ** argv) {
	const int ARRAY_SIZE = 64;
	const int ARRAY_BYTES = ARRAY_SIZE * sizeof(float);

	// generate the input array on the host
	float h_in[ARRAY_SIZE];
	for (int i = 0; i < ARRAY_SIZE; i++) {
		h_in[i] = float(i);
	}
	float h_out[ARRAY_SIZE];

	//declare GPU memory pointers
	float * d_in;
	float * d_out;

	//allocate GPU memory
	cudaMalloc((void **) &d_in, ARRAY_BYTES);
	cudaMalloc((void **) &d_out, ARRAY_BYTES);

	//transfer the array to the GPU
	cudaMemcpy(d_in, h_in, ARRAY_BYTES, cudaMemcpyHostToDevice);

	//launch the kernel
	square<<<1, ARRAY_SIZE>>>(d_out, d_in);

	//copy back the result array to the CPU
	cudaMemcpy(h_out, d_out, ARRAY_BYTES, cudaMemcpyDeviceToHost);

	//print out the resulting array
	for (int i = 0; i < ARRAY_SIZE; i++) {
		printf("%f", h_out[i]);
		printf(((i % 4) != 3) ? "\t" : "\n");
    }
    //free GPU memory allocation
    cudaFree(d_in);
    cudaFree(d_out);

    return 0;	
}
```

After running it, you will get the following result :

```
jundeMacBook-Pro:CUDAprogramming jun$ ./square
0.000000	1.000000	4.000000	9.000000
16.000000	25.000000	36.000000	49.000000
64.000000	81.000000	100.000000	121.000000
144.000000	169.000000	196.000000	225.000000
256.000000	289.000000	324.000000	361.000000
400.000000	441.000000	484.000000	529.000000
576.000000	625.000000	676.000000	729.000000
784.000000	841.000000	900.000000	961.000000
1024.000000	1089.000000	1156.000000	1225.000000
1296.000000	1369.000000	1444.000000	1521.000000
1600.000000	1681.000000	1764.000000	1849.000000
1936.000000	2025.000000	2116.000000	2209.000000
2304.000000	2401.000000	2500.000000	2601.000000
2704.000000	2809.000000	2916.000000	3025.000000
3136.000000	3249.000000	3364.000000	3481.000000
3600.000000	3721.000000	3844.000000	3969.000000
```

*Note:* To run CUDA applications on MacBook Pro with both an integrated GPU and a discrete GPU, use the following settings to switch your Graphics to discrete GPU:

- Uncheck System Preferences > Energy Saver > Automatic Graphic Switch
- Drag the Computer sleep bar to Never in System Preferences > Energy Saver

#### **3.6 Sample example in CUDA**

To fully verify that the compiler works properly, you can also use samples NVIDIA provided. After switching to the directory where the samples were installed, (For example: `cd /Developer/NVIDIA/CUDA-7.5/samples/`) then type:

```
make -C 0_Simple/vectorAdd
make -C 0_Simple/vectorAddDrv
make -C 1_Utilities/deviceQuery
make -C 1_Utilities/bandwidthTest
```

After compile these samples, you can run them. 
Switching to the directory `cd /Developer/NVIDIA/CUDA-7.5/samples/bin/x86_64/darwin/release`, then run

```
jundeMacBook-Pro:release jun$ ./vectorAdd 
[Vector addition of 50000 elements]
Copy input data from the host memory to the CUDA device
CUDA kernel launch with 196 blocks of 256 threads
Copy output data from the CUDA device to the host memory
Test PASSED
Done
```

```
jundeMacBook-Pro:release jun$ ./bandwidthTest 
[CUDA Bandwidth Test] - Starting...
Running on...

 Device 0: GeForce GT 750M
 Quick Mode

 Host to Device Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)	Bandwidth(MB/s)
   33554432			1818.5

 Device to Host Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)	Bandwidth(MB/s)
   33554432			3253.1

 Device to Device Bandwidth, 1 Device(s)
 PINNED Memory Transfers
   Transfer Size (Bytes)	Bandwidth(MB/s)
   33554432			17442.4

Result = PASS

NOTE: The CUDA Samples are not meant for performance measurements. Results may vary when GPU Boost is enabled.
```

You can find something interesting from the result. HAHA, enjoy it~

```
jundeMacBook-Pro:release jun$ ./deviceQuery 
./deviceQuery Starting...

 CUDA Device Query (Runtime API) version (CUDART static linking)

Detected 1 CUDA Capable device(s)

Device 0: "GeForce GT 750M"
  CUDA Driver Version / Runtime Version          7.5 / 7.5
  CUDA Capability Major/Minor version number:    3.0
  Total amount of global memory:                 2048 MBytes (2147024896 bytes)
  ( 2) Multiprocessors, (192) CUDA Cores/MP:     384 CUDA Cores
  GPU Max Clock rate:                            926 MHz (0.93 GHz)
  Memory Clock rate:                             2508 Mhz
  Memory Bus Width:                              128-bit
  L2 Cache Size:                                 262144 bytes
  Maximum Texture Dimension Size (x,y,z)         1D=(65536), 2D=(65536, 65536), 3D=(4096, 4096, 4096)
  Maximum Layered 1D Texture Size, (num) layers  1D=(16384), 2048 layers
  Maximum Layered 2D Texture Size, (num) layers  2D=(16384, 16384), 2048 layers
  Total amount of constant memory:               65536 bytes
  Total amount of shared memory per block:       49152 bytes
  Total number of registers available per block: 65536
  Warp size:                                     32
  Maximum number of threads per multiprocessor:  2048
  Maximum number of threads per block:           1024
  Max dimension size of a thread block (x,y,z): (1024, 1024, 64)
  Max dimension size of a grid size    (x,y,z): (2147483647, 65535, 65535)
  Maximum memory pitch:                          2147483647 bytes
  Texture alignment:                             512 bytes
  Concurrent copy and kernel execution:          Yes with 1 copy engine(s)
  Run time limit on kernels:                     Yes
  Integrated GPU sharing Host Memory:            No
  Support host page-locked memory mapping:       Yes
  Alignment requirement for Surfaces:            Yes
  Device has ECC support:                        Disabled
  Device supports Unified Addressing (UVA):      Yes
  Device PCI Domain ID / Bus ID / location ID:   0 / 1 / 0
  Compute Mode:
     < Default (multiple host threads can use ::cudaSetDevice() with device simultaneously) >

deviceQuery, CUDA Driver = CUDART, CUDA Driver Version = 7.5, CUDA Runtime Version = 7.5, NumDevs = 1, Device0 = GeForce GT 750M
Result = PASS
```




## 4. OpenCV with CUDA
jing qing qi dai, xie xie

## 5. FAQ
I met some problems after installation. I have google them but there is no specific answer for these questions. So I decided to write down my solutions for them. Hope this can help you and save your time.

---------

#### ***Q1:  When complie the CUDA samples, report error as below*** 

```
error: unable to open output file 'vectorAdd.o': 'Permission denied'
1 error generated.
make: *** [vectorAdd.o] Error 1
```

***A1:***  The CUDA samples' directory is not writable. You just need to add `sudo` in front of your command like this `sudo make -C 0_Simple/vectorAdd`. Alternatively, you can copy the CUDA samples files to your home directory, then compile them there.

---------

#### ***Q2:  Still Cannot find "nvcc" commend after install the CUDA? like this*** `nvcc: command not found`

***A2:*** Actually you have installed the CUDA driver, CUDA Toolkit and CUDA samples after finishing all the steps above. But you didn't add the path of NVCC to your system. Just like the PATH in Matlab, if the function is not in your current path, you cannot call it. So you cannot call `nvcc` complier without specifying the full path of `nvcc` in the Terminal.

*How to add the PATH?*

Actually, there are totally 3 methods for Bourne Shell environment variable configuration. Remember to make sure your Mac is Bourne Shell, check it using `echo $SHELL`. If the output is `bash, zsh, sh`, you are using  a kind of Bourne Shell. 

-  `~/.bash_profile` is executed every time user log in. It is only read by bash.
-  `/etc/bashrc` is for environment variable of system.  It is executed on each instance of the shell.
-  `/etc/profile` is for things that are not specifically related to Bash. Every time you log in, Bash will source `.bash_profile`first, if that doesn't exist, it will source `.profile`

***The recommended way is by editing your `.bash_profile` file. This file is read and the commands in it executed by Bash every time you log in to the system. The following steps I collated are how to add PATH to `.bash_profile`.***

- You can check your path like this: `echo $PATH`
- You will get result like this. There are totally 7 default paths. However, there is no path for CUDA!  That's why you cannot find nvcc command.

```
/Users/jun/anaconda/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Library/TeX/texbin
```
- If you have created a `.bash_profile` before, you only need to enter  `open ~/.bash_profile`. 
Else, you need to create a bash_profile at first, then open it. Like this`touch ~/.bash_profile; open ~/.bash_profile`
- Write the code below in this file:

```
export LD_LIBRARY_PATH=/usr/local/cuda/lib
export PATH=$PATH:/usr/local/cuda/bin
```
- Then save and logout. This will add the PATH of nvcc to your system path automatically every time you login your terminal.
- After relaunched your terminal ! Use `nvcc -V` You will find result like this, it illustrates your `nvcc`command is available: 

```
nvcc: NVIDIA (R) Cuda compiler driver 
Copyright (c) 2005-2015 NVIDIA Corporation
```

- You can also check it like this `echo $PATH`, you will find a new path of CUDA at last. Congratulations! You make it!

```
/Users/jun/anaconda/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/Library/TeX/texbin:/usr/local/cuda/bin
```


## 6. Reference

[NVIDIA CUDA documentation](http://docs.nvidia.com/cuda/cuda-getting-started-guide-for-mac-os-x/index.html#installation)

[Stack Overflow](http://stackoverflow.com/)

[Udacity-Parallel programming](https://www.udacity.com/)

