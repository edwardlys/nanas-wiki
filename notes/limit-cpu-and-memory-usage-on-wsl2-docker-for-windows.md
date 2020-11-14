---
title: Limit CPU and memory usage on WSL2 + Docker for Windows
description: 
published: true
date: 2020-11-14T05:30:30.949Z
tags: cpu, memory, docker, wsl2
editor: markdown
dateCreated: 2020-11-14T04:25:55.110Z
---

## Spotting the issue
Once you have fired up Docker for Windows on your Windows machine, check the task manager and look for a process named "Vmmem" which roughly stands for VM memory or memory allocation for VM.

The reason is, Docker only runs on Linux kernels. Docker for Windows relies on WSL2 which uses Hyper-V virtual machine technology. 

Online resources shows that WSL2 allocates memory and CPU resources dynamically. However from my experiences, WSL2 does allocate more memory when it needs to but I have never seen it releasing its memory. Only after stopping the Docker engine and WSL2 entirely for a few minutes, this memory allocation will be freed once again.

However, you are like me and running a little short on RAM, it can lead to a sluggish machine.

Coupling this issue that with 50 tabs of code documentations and YouTube video tutorials in Chrome browser (as all developers do), you can find yourself running out of memory almost too soon.

## How to limit memory and CPU

One can always limit the CPU and memory usage of this VM.

At the directory `C:\Users\<username>`, edit or create a file named `.wslconfig` with the contents below:

```
[wsl2]
memory=1GB 		# Limits VM memory in WSL 2 to 1GB
processors=2 	# Makes the WSL 2 VM use two virtual processors
```
There is more to limitting the memory and processor in `.wslconfig`. More options can be found here: https://docs.microsoft.com/en-us/windows/wsl/wsl-config#wsl-2-settings