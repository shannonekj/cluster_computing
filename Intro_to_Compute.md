# Intro to Compute


## What is a cluster?

A cluster can be thought of as a group of computers which work together to allow you to log onto one computer (**head node**) and use resources to perform memory intensive functions from other connected computers.

center><img src="https://i.imgur.com/2nl5zzP.png" width="50%"></a></center>
Image modified from (http://www.vrlab.umu.se/documentation/guides/beginner-guide)

## How do clusters work?



### Job Schedulers

Job schedulers handle how to allocate the compute cluster's resources to batch jobs submitted by users. The job scheduler evaulates when resources will be dedicated to a job based on the:

* job priority
* group's resources already taken up
* wall time requested
* resources requested


** Slurm ** 

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

** General Use **

* space left for group

```
df -h | grep <username>
```

* how much space you are taking up

```
du -d 0 -h
``` 
