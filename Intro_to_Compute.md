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
* job's priority (`-p`)
* group's resources already taken up
* requested wall time (`-t`)
* requested resources
    * memory (`--mem`)
    * CPUs (`-c`)


**Slurm** [workload manager](https://slurm.schedmd.com/documentation.html)

Slurm is an open source workload manager that is commonly used on compute clusters (both the FARM and barbera use Slurm). It handles allocating resources requested by batch scripts. 

There are **two** main ways you can request resources using Slurm:

#### 1. Run an interactive session with `srun`

Interactive sessions allow you to work on computers that aren't the login/head node. Essentially you can do everything you've done at the command line interface on Jetstream on the compute cluster. This is really powerful for doing memory intensive commands that you may not need to keep track of. However, with this power comes a great danger as the commands you run will not be save in a script anywhere. So, if you wanted to go back and recreate an analysis, you won't know what you've run or with which versions of software.

To request and launch a basic interactive session that will last for two hours use the following:
```
srun --time=02:00:00 --pty /bin/bash
```

Pay close attention to the time you give to yourself using `srun`! Slurm will terminate the session immediately at the end of the allotted time. It, sadly, doesn't care if you are 99.99% of the way through your analysis :0]

Also, you can request more/different resources by using to following flags:
* `--mem=<number>Gb` = request memory
* `-c <number>` = request a certain number of CPUs
* `--pty R` = request an interactive R session


#### 2. Submit batch scripts with `sbatch`

Batch job scripts (also known as job scripts) are scripts that contain `#!/bin/bash` at the beginning of each script and are submitted to the slurm workload manager by using `sbatch`. When we submit a script to slurm it is considered a _job_ and gets a unique job ID assigned to it.

We can see an example batch script in the **HelloWorld.sh** file
```
cat HelloWorld.sh
```
```
#!/bin/bash

echo Hello World
date
```

We can submit this script to **Slurm** with the `sbatch` command.
```
sbatch HelloWorld.sh
```
but we receive an error message...
```
sbatch: error: Batch job submission failed: Requested time limit is invalid (missing or exceeds some limit)
```
In order to handle jobs, Slurm needs to know the maximum amount of **walltime** your job will run. Walltimecan be thought of as the amount of time from the start of your code running to when it finishes. We can tell Slurm how much time to dedicate to our submitted script by using the `-t` flag. Let's tell Slurm that our job will take no more than 5 minutes (note: the format is `dd-hh-mm-ss`.

```
sbatch -t 00-00:05:00 HelloWorld.sh
``` 
You will see your job was successfully submitted and an associated Job ID number `Submitted batch job 15219016`


#### Monitor your jobs with `squeue`

Oftentimes we submit jobs and would like to know certain things about them -- if they've started, how long they've been running, if they are still running, etc, etc... We can look at the status of any job Slurm is handling by using `squeue`

Let's submit the `HoldWorld.sh` to slurm:
```
sbatch -t 00-01:10:00 HoldWorld.sh
```
If only type
```
squeue
```
```
         JOBID PARTITION     NAME     USER ST        TIME  NODES CPU MIN_ME NODELIST(REASON)
      15218450       bmh this_is_ keyu1996 CG       31:10      1 2   100G   bm3
      15219413       bmh     pigz   aminio CG        0:01      1 8   4G     bm2
15108157_[34-4   bigmemm  mapping gfburgue PD        0:00      1 8   200G   (Resources)
14204771_[1182       med freebaye eoziolor PD        0:00      1 4   2000M  (AssocGrpCpuLimit)
15217722_[7-23       bmm     trim hansvgdu PD        0:00      1 2   10G    (JobArrayTaskLimit)
      15113687   bigmemm AA_ophiu jgillung PD        0:00      1 24  200G   (Priority)
      15144078   bigmemm NT_ophiu jgillung PD        0:00      1 24  200G   (Priority)
      15144205   bigmemm AA_plant jgillung PD        0:00      1 24  200G   (Priority)
      15144210   bigmemm NT_plant jgillung PD        0:00      1 24  200G   (Priority)
```
then we see **ALL** the jobs currently submitted to Slurm -- which usually quite a few! And often we won't be able to just scroll through the list to find our job(s). So, in order to only see your own jobs we can specify our username:
```
squeue -u <username>
```
```

```


#### Cancel your jobs with `scancel`



#### Repeatibility

One of the most important things in science is repeatability. This sentiment holds true in bioinformatics experiments as well. However, it is exceptionally easy to run a series of command on data, leave the data for a few months (or years) and come back to the data and have no clue how you went from point A to point Z. It is hard, if not impossible to recreate an analysis with exactly the same string of commands and parameters so we should think about documenting things as we go. In the worst case scenario you have to recreate an analysis and in the best case of forgetting to document commands, you may have to tweak how you submit your job scripts to slurm. 

We can make this easy for our _future_ forgetful-selves and put all of the flags we submit our Slurm jobs with INSIDE our batch scripts!

Take a look at the `HelloUniverse.sh` script
```
cat HelloUniverse.sh
```

```
#!/bin/bash
#
#SBATCH --mail-user=sejoslin@ucdavis.edu        # YOUR EMAIL ADDRESS
#SBATCH --mail-type=ALL                         # NOTIFICATIONS OF SLURM JOB STATUS
#SBATCH -J HelloUni                             # JOB ID
#SBATCH -e HelloUniverse.j%j.err                # STANDARD ERROR FILE TO WRITE TO
#SBATCH -o HelloUniverse.j%j.out                # STANDARD OUTPUT FILE TO WRITE TO
#SBATCH -c 4                                    # NUMBER OF PROCESSORS PER TASK
#SBATCH --ntasks=8                              # MINIMUM NUMBER OF NODES TO ALLOCATE TO JOB
#SBATCH --mem=4gb                               # MEMORY POOL TO ALL CORES
#SBATCH --time=01-00:00:48                      # REQUESTED WALL TIME
#SBATCH -p high                                 # PARTITION TO SUBMIT TO

echo Hello Universe
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
