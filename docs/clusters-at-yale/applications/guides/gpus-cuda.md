# GPUs and CUDA

We currently have GPUs available for general use on [Grace](/clusters-at-yale/clusters/grace) and [Farnam](/clusters-at-yale/clusters/farnam). See cluster pages for hardware and queue/partition specifics.

## Access the GPU Nodes

To access the GPU nodes you must request them with the scheduler.

An example Slurm command to request an interactive job on the gpu partition with X forwarding and 1/2 of a GPU node (10 cores and 1 K80):

``` bash
srun --pty --x11 -p gpu -c 10 -t 24:00:00 --gres=gpu:2 --gres-flags=enforce-binding bash
```

The `--gres=gpu:2` option asks for two gpus, and the `--gres-flags=enforce-binding` option ensures you get two GPUs on the same card, and that the CPUs you are allocated are on the same bus as your GPU.

To submit a batch job, include the following directives (in addition to your core, time, etc requests):

``` bash
#SBATCH -p gpu
#SBATCH --gres=gpu:1
#SBATCH --gres-flags=enforce-binding
```

!!!warning
    Please do not use nodes with GPUs unless your application or job can make use of them. Any jobs found running in a GPU partition without a GPU will be terminated without warning.

You can check the available GPUs and their current usage with the command [nvidia-smi](https://developer.nvidia.com/nvidia-system-management-interface).

## Software

CUDA and cuDNN are available as modules where applicable. On your cluster of choice use toolkit is installed on the GPU nodes. To see what is available, type:

``` bash
module avail cuda
```

## Drivers

The CUDA libraries you load will allow you to compile code against them. To run CUDA-enabled code you must also be running on a node with a gpu allocated and a compatible driver installed. The minimum driver versions are as follows ([borrowed from this post](https://stackoverflow.com/questions/30820513/what-is-version-of-cuda-for-nvidia-304-125/30820690#30820690)):

```
CUDA 9.2: 396.xx
CUDA 9.1: 387.xx
CUDA 9.0: 384.xx
CUDA 8.0: 375.xx
CUDA 7.5: 352.xx
CUDA 7.0: 346.xx
CUDA 6.5: 340.xx
CUDA 6.0: 331.xx
```

To check the insalled version of the nvidia drivers, you can also use `nvidia-smi`:

``` bash
[user@gpu01 ~]$ nvidia-smi 
Tue Jan 16 10:31:45 2018       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 375.66                 Driver Version: 375.66                    |
...
```

Here we see that the node gpu01 is running driver version 375.66\. It can run CUDA 8 but not CUDA 9 yet.