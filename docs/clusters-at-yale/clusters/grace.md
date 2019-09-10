# Grace

![Grace](/img/Grace-Hopper.jpg){: .cluster-portrait}

The Grace cluster is is named for the computer scientist and United States Navy Rear Admiral [Grace Murray Hopper](https://en.wikipedia.org/wiki/Grace_Hopper), who received her Ph.D. in Mathematics from Yale in 1934.

Grace is a shared-use resource for the [Faculty of Arts and Sciences](https://fas.yale.edu) (FAS). It consists of a variety of compute nodes networked over low-latency InfiniBand and mounts several shared filesystems.

- - -

## Hardware

Grace is made up of several kinds of compute nodes. The Features column below lists the features that can be used to request different node types using the `--constraints` flag (see our [Slurm documentation](/clusters-at-yale/job-scheduling/resource-requests#features-and-constraints) for more details). The RAM listed below is the amount of memory available for jobs. GPUs listed can be requested with the `--gres` flag, e.g. `--gres=gpu:p100:1` would request one Tesla P100 GPU per node. See the [Request Compute Resources page](clusters-at-yale/job-scheduling/resource-requests/#request-gpus) for more info.

!!! Warning
    Care should be taken if when scheduling your job if you are running programs/libraries optimized for specific hardware.
    See the [guide on how to compile software](/clusters-at-yale/applications/compile) for specific guidance.

### Compute Node Configurations

| Count | CPU                 | CPU Cores | RAM   |         GPU        | Features                                       |
|-------|---------------------|-----------|-------|--------------------|------------------------------------------------|
| 80    | 2x E5-2660 v2       | 20        | 121G  |                    | ivybridge, E5-2660_v2, nogpu, standard, oldest |
| 136   | 2x E5-2660 v3       | 20        | 121G  |                    | haswell, avx2, E5-2660_v3, nogpu, standard     |
| 20    | 2x E5-2660 v3       | 20        | 247G  |                    | haswell, avx2, E5-2660_v3, nogpu, standard     |
| 6     | 2x E5-2660 v3       | 20        | 121G  | 2x k80 (2GPUs/k80) | haswell, avx2, E5-2660_v3, doubleprecision     |
| 8     | 2x E5-2660 v3       | 20        | 247G  | 1x k80 (2GPUs/k80) | haswell, avx2, E5-2660_v3, doubleprecision     |
| 161   | 2x E5-2660 v4       | 28        | 247G  |                    | broadwell, avx2, E5-2660_v4, nogpu, standard   |
| 7     | 2x E5-2660 v4       | 28        | 247G  | 1x p100            | broadwell, avx2, E5-2660_v4, doubleprecision   |
| 1     | 2x E5-2637 v4       | 8         | 121G  | 4x gtx1080ti       | broadwell, avx2, E5-2637_v4, singleprecision   |
| 1     | 2x Gold 6126        | 24        | 176G  |                    | skylake, avx2, avx512, 6126, nogpu, standard   |
| 53    | 2x Gold 6136        | 24        | 90G   |                    | skylake, avx2, avx512, 6136, nogpu, standard   |
| 1     | 2x Gold 6136        | 24        | 90G   | 2x v100            | skylake, avx2, avx512, 6136, doubleprecision   |
| 9     | 2x Gold 6136        | 24        | 183G  | 4x p100            | skylake, avx2, avx512, 6136, doubleprecision   |
| 3     | 2x Gold 6142        | 36        | 183G  |                    | skylake, avx2, avx512, 6142, nogpu, standard   |
| 2     | 2x Gold 5122        | 8         | 183G  | 4x rtx2080         | skylake, avx2, avx512, 5122, singleprecision   |
| 1     | 4x E7-4820 v2       | 32        | 1003G |                    | ivybridge, E7-4820_v2, nogpu                   |
| 1     | 4x E7-4809 v2       | 32        | 2011G |                    | haswell, avx2, E7-4809_v3, nogpu               |
| 4     | 4x E7-4820 v4       | 40        | 1507G |                    | broadwell, avx2, E7-4820_v4, nogpu             |
| 1     | 2x Gold 6136        | 24        | 751G  |                    | skylake, avx2, avx512, 6136, nogpu             |

## Slurm Partitions

Nodes on the clusters are organized into partitions, to which you submit your jobs with [Slurm](/clusters-at-yale/job-scheduling). The default resource requests for all jobs is 1 core and 5GB of memory, unless otherwise specified.

### Public Partitions

The day partition is where most batch jobs should run, and is the default if you don't specify a partition. The week partition is smaller, but allows for longer jobs. The interactive partition should only be used for testing or compiling software. The bigmem partition contains our largest memory node; only jobs that cannot be satisfied by general should run here. The gpu_devel partition is a single node meant for testing or compiling GPU accelerated code, and the gpu partition is where normal GPU jobs should run. The mpi partition is reserved for tightly-coupled parallel and access is by special request only. The scavenge partition allows you to run preemptable jobs on more resources than normally allowed. For more information about scavenge, see the [Scavenge documentation](/clusters-at-yale/job-scheduling/scavenge).

The limits listed below are for all running jobs combined. Per-node limits are bound by the node types, as described in the [hardware](#hardware) table.

| Partition   | Group Limits | User Limits                 | Walltime Default/Max | Node Type (count)                                      |
|-------------|--------------|-----------------------------|----------------------|--------------------------------------------------------| 
| day*        | 900 CPUs     | 640 CPUs                    | 1h/1d                | E5-2660_v2 (52), E5-2660_v3 (32), E5-2660_v4 (72)      |
| week        | 250 CPUs     | 100 CPUs                    | 1h/7d                | E5-2660_v3 (48), E5-2660_v4 (7)                        |
| interactive |              | 1 job, 4 CPUs, 32 G RAM     | 1h/6h                | E5-2660_v2 (2), 6126 (1)                               |
| bigmem      |              | 40 CPUs, 1500 G RAM         | 1h/1d                | E7-4820_v4 1507G (2)                                   |
| gpu_devel   |              | 1 job, 10 CPUs, 60 G RAM    | 10min/4hr            | E5-2660_v3 k80 (1)                                     |
| gpu         |              | 12 GPUs                     | 1h/1d                | E5-2660_v3 k80 (5), E5-2660_v4 p100 (6), 6136 v100 (1) |
| mpi**       |              | 18 nodes                    | 1h/1d                | 6136 (33)                                              |
| scavenge    |              | 6400 CPUs                   | 1h/1d                | all                                                    |
| scavenge_gpu|              | 20 GPUs                     | 1h/1d                | all nodes with GPUs (see Compute Node table)           |

\* default  
** The mpi partition is reserved for tightly-coupled parallel programs that make efficient use of multiple nodes. Contact us at [hpc@yale.edu](mailto:hpc@yale.edu) for access if your workload fits this description. The default memory request on the mpi partition in 3.75GB per core.

### Private Partitions

Private partitions contain nodes acquired by specific research groups. Full access to these partitions is granted at the discretion of the owner. Contact us if your group would like to purchase nodes.

| Partition           | Walltime Default/Max | Node Type (count)                                                      |
|---------------------|----------------------|------------------------------------------------------------------------|
| pi_altonji          | 1d/28d               | E5-2660_v3 (2)                                                         |
| pi_anticevic        | 1d/100d              | E5-2660_v3 (16), E5-2660_v4 (20)                                       |
| pi_anticevic_bigmem | 1d/100d              | E7-4809_v3 2011G (1)                                                   |
| pi_anticevic_fs     | 1d/100d              | E5-2660_v3 (3)                                                         |
| pi_anticevic_gpu    | 1d/100d              | E5-2660_v3 k80 (8)                                                     |
| pi_balou            | 1d/28d               | E5-2660_v4 (30)                                                        |
| pi_berry            | 1d/28d               | E5-2660_v3 (1)                                                         |
| pi_cowles           | 1d/28d               | E5-2660_v3 (14)                                                        |
| pi_cowles_nopreempt | 1d/28d               | E5-2660_v3 (10)                                                        |
| pi_gelernter        | 1d/28d               | E5-2660_v4 (1)                                                         |
| pi_gerstein         | 1d/28d               | E5-2660_v3 (32), E7-4820_v2 1003G (1)                                  |
| pi_glahn            | 1d/100d              | E5-2660_v3 (1)                                                         |
| pi_hammes_schiffer* | 1d/28d               | 6136 (16), 6136 751G (1), 5122 gtx1080ti (1), E5-2637_v4 gtx1080ti (2) |
| pi_holland          | 1d/28d               | E5-2660_v3 (2)                                                         |
| pi_jetz             | 1d/28d               | E5-2660_v4 (2)                                                         |
| pi_kaminski         | 1d/28d               | E5-2660_v3 (8)                                                         |
| pi_mak              | 1d/28d               | E5-2660_v4 (8)                                                         |
| pi_manohar          | 1d/180d              | E5-2660_v4 (8), E5-2660_v4 p100 (1), E7-4820_v2 1507G (2)              |
| pi_ohern            | 1d/28d               | E5-2660_v2 (16), E5-2660_v4 (3), 6136 p100 (9)                         |
| pi_owen_miller      | 1d/28d               | E5-2660_v4 (5)                                                         |
| pi_poland           | 1d/28d               | E5-2660_v4 (10)                                                        |
| pi_seto             | 1d/28d               | 6142 (3)                                                               |
| pi_tsmith           | 1d/28d               | E5-2660_v3 (1)                                                         |

\* The default memory request on this partition is 3.75GB per core.  

## Storage

Grace has access to a number of GPFS filesystems. `/gpfs/loomis` is Grace's primary filesystem where home, project, and scratch60 directories are located. For more details on the different storage spaces, see our [Cluster Storage](/clusters-at-yale/data/cluster-storage) documentation.

You can check your current storage usage & limits by running the `getquota` command. Note that the per-user usage breakdown only update once daily.

!!! Warning
    Files stored in `scratch60` are purged if they are older than 60 days. You will receive an email alert one week before they are deleted.

|Partition  | Root Directory            | Storage     | File Count | Backups |
|-----------|---------------------------|-------------|------------|---------|
| home      | `/gpfs/loomis/home.grace` | 100G/user   | 200,000    | Yes     |
| project   | `/gpfs/loomis/project`    | 1T/group    | 5,000,000  | No      |
| scratch60 | `/gpfs/loomis/scratch60`  | 20T/group   | 5,000,000  | No      |