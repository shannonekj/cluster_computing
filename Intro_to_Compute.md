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
    * explain flags (internal & external)
        * c
