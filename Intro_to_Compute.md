## General Information
# What is a cluster?
# How do clusters work?
* General structure
    * head node
    * job schedulers
* slurm
    * explain structure of Slurm
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
* general use
    * space left for group
```
df -h | grep <username>
```
    * how much space you are taking up
```
du -d 0 -h
``` 
