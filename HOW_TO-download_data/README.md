# How to download data _to_ and _from_ clusters


## From SLIMS --> the farm

To download data off of SLIMS open up a web browser and [log into SLIMS](https://slims.bioinformatics.ucdavis.edu/) using your username and password. 

This should open the "View Runs" page of the website. Find the run that belongs to you (they are organized by date) and click the **view** button on the righthand side of the row. This will give you more information on that specific run. The 'Submitted' date should match the date you (or your PI) received an email from the DNA Technologies Core. The description should be a description that matches the data you expect. 

After honing in on which line is yours click the **All Files** link at the far right end of the line. This will take you to where your files are housed on SLIMS. 

Keep that page open and log onto the farm in a terminal window. 

We are going to use a batch script to download the data. Please note that the `sbatch` options the slurm requires varies from cluster to cluster. This set of directions are for downloading to _the farm_.



```
#!/bin/bash
#SBATCH --mail-user=<your_email>
#SBATCH --mail-type=ALL
#SBATCH -J download
#SBATCH -e download_data.err
#SBATCH -o download_data.out
#SBATCH -c 20
#SBATCH -p high
#SBATCH --time=03:00:00
#SBATCH --mem MaxMemPerNode

set -e
set -x

data_dir="</global/path/to/where/you/want/to/store/data>"
slims_url="<the_url_to_your_slims_data>"

mkdir -p ${data_dir}
cd ${data_dir}

echo Starting at $(date +%D' '%T)
wget -r -nH -nc -np -R index.html* ${slims_url}
echo Download completed at $(date +%D' '%T)
```

Replace everything between these: < >

There are items to replace on:
* line 2
* line 15
* line 16
* line 21

For example, my 15th line would read `data_dir="/projects/genome-Delta_smelt/00-raw_data` to store my sequencing data in the raw data directory of my Delta smelt genome project. :)



## Future "how to's"
* From the farm --> your CPU
* From barbera --> farm
* From the farm --> a box account

Please open an **issue** with this repository to request more resources on how to move data around.
