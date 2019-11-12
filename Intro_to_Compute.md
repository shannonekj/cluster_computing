# Intro to Compute


## What is a cluster?

A cluster can be thought of as a group of computers which work together to allow you to log onto one computer (**head node**) and use resources to perform memory intensive functions from other connected computers.

<center><img src="https://i.imgur.com/2nl5zzP.png" width="50%"></a></center>
Image modified from (http://www.vrlab.umu.se/documentation/guides/beginner-guide)

## How do clusters work?



### Job Schedulers

In order to carry out commands that are memory intensive we need to use auxillary computers that will not affect the login/head node. As a note, sometimes merely copying large files is memory intensive enough that we will need to use other computers! To request resources to run our scripts we use job schedulers. Job schedulers handle how to allocate the compute cluster's resources to batch job scripts submitted by users.

There are a number of different flavors of job schedulers specific to the cluster you will use through your institution but they all have the following general structure:

![](https://i.imgur.com/9rSbIxR.png)

The job scheduler evaulates when resources will be dedicated to a job based on the:
* job's priority (**-p**)
* group's resources already taken up
* requested wall time (**-t**)
* requested resources
    * memory (**--mem**)
    * CPUs (**-c**)


**Slurm** [workload manager](https://slurm.schedmd.com/documentation.html)

Slurm is an open source workload manager that is commonly used on compute clusters (both the FARM and barbera use Slurm). It handles allocating resources requested by batch scripts. 

There are **two** main ways you can request resources using Slurm:

#### 1. Run an interactive session.

Interactive sessions allow you to work on computers that aren't the login/head node. Essentially you can do everything you've done at the command line interface on Jetstream on the compute cluster. This is really powerful for doing memory intensive commands that you may not need to keep track of. However, with this power comes a great danger as the commands you run will not be save in a script anywhere. So, if you wanted to go back and recreate an analysis, you won't know what you've run or with which versions of software.

To request and launch a basic interactive session that will last for two hours use the following:
```
srun --time=02:00:00 --pty /bin/bash
```

Pay close attention to the time you give to yourself using `srun`! Slurm will terminate the session immediately at the end of the allotted time. It, sadly, doesn't care if you are 99.99% of the way through your analysis :0]

Also, you can request more/different resources by using to following flags:
* `--mem=<number>Gb` request memory
* `-c <number>` request a certain number of CPUs


#### 2.Submit batch scripts with `sbatch`

Batch job scripts (also known as job scripts) are scripts that contain `#!/bin/bash` at the beginning of each script and are submitted to the slurm workload manager by using `sbatch`. When we submit a script to slurm it is considered a _job_ and gets a unique job ID assigned to it.

We can see an example batch script in the **helloworld.sh** file
```
cat helloworld.sh
```
```
#!/bin/bash

echo Hello World
date
```

 

* commands
    * sbatch -- Command to submit a job (batch script) to the Slurm scheduler
        * cpu
        * nodes
        * partitions
        * memory
    * sinfo -- Commnd to get information about Slurm nodes and partiions
        * PARTITION
        * AVAIL
        * TIMELIMIT
        * NODES
        * STATE
        * NODELIST
        * -p
    * squeue -- Command to view information about jobs located in the Slurm scheduling queue.
        * -A
        * -u
        * -p
    * srun -- Command to run parallel jobs
    * scancel -- Command to cancel jobs submitted to Slurm
        * by job number
        * by username

**General Use**

* space left for group
```
df -h | grep <username>
```
* how much space you are taking up
```
du -d 0 -h
``` 
