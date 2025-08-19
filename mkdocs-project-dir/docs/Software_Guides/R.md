---
title: R
layout: docs
---

Type `module avail r` to see the currently available versions of R.

The current version will always also exist as `r/recommended` - this is a module bundle and loading it will also load its many dependencies.

`module show r/recommended` shows you exactly which versions loading this module will give you.

R can be run on a single core or multithreaded using many cores (some commands can run threaded automatically, otherwise you may wish to look at R's `parallel` package). 

`doMPI`, `Rmpi` and `snow` allow multi-node parallel jobs using MPI to be run.

[List of additional R packages](../Installed_Software_Lists/r-packages.md) shows you what packages are installed and available for the current R version.

## Setup

Before you can use R interactively, you need to load the R module using: 

```
module -f unload compilers mpi gcc-libs
module load r/recommended
```

## Example serial jobscript

This script runs R using only one core.

```
#!/bin/bash -l

# Example jobscript to run a single core R job

# Request ten minutes of wallclock time (format hours:minutes:seconds).
# Change this to suit your requirements.
#$ -l h_rt=0:10:0

# Request 1 gigabyte of RAM. Change this to suit your requirements.
#$ -l mem=1G

# Set the name of the job. You can change this if you wish.
#$ -N R_job_1

# Set the working directory to somewhere in your scratch space.  This is
# necessary because the compute nodes cannot write to your $HOME
# NOTE: this directory must exist.
# Replace "<your_UCL_id>" with your UCL user ID
#$ -wd /home/<your_UCL_id>/Scratch/R_output

# Your work must be done in $TMPDIR (serial jobs particularly) 
cd $TMPDIR

# Load the R module and run your R program
module -f unload compilers mpi gcc-libs
module load r/recommended

R --no-save < /home/username/Scratch/myR_job.R > myR_job.out

# Preferably, tar-up (archive) all output files to transfer them back 
# to your space. This will include the R_output file above.
tar zcvf $HOME/Scratch/R_output/files_from_job_$JOB_ID.tgz $TMPDIR

# Make sure you have given enough time for the copy to complete!
```

You will need to change the `-wd /home/<your_UCL_id>/Scratch/R_output` location and the location of your R input file, called `myR_job.R` here.  `myR_job.out` is the file we are redirecting the output into. The output file is saved in the tar archive produced by the last command in the runscript and will be in `$HOME/Scratch/R_output`. 

If your jobscript is called `run-R.sh` then your job submission command would be:
```
qsub run-R.sh
``` 

## Example shared memory threaded parallel job

This script uses multiple cores on the same node. It cannot run across multiple nodes.

```
#!/bin/bash -l

# Example jobscript to run an OpenMP threaded R job across multiple cores on one node.
# This may be using the foreach packages foreach(...) %dopar% for example.

# Request ten minutes of wallclock time (format hours:minutes:seconds).
# Change this to suit your requirements.
#$ -l h_rt=0:10:0

# Request 1 gigabyte of RAM per core. Change this to suit your requirements.
#$ -l mem=1G

# Set the name of the job. You can change this if you wish.
#$ -N R_jobMC_2

# Select 12 threads. The number of threads here must equal the number of worker 
# processes in the registerDoMC call in your R program.
#$ -pe smp 12

# Set the working directory to somewhere in your scratch space.  This is
# necessary because the compute nodes cannot write to your $HOME
# NOTE: this directory must exist.
# Replace "<your_UCL_id>" with your UCL user ID
#$ -wd /home/<your_UCL_id>/Scratch/R_output

# Your work must be done in $TMPDIR
cd $TMPDIR

# Load the R module and run your R program
module -f unload compilers mpi gcc-libs
module load r/recommended

R --no-save < /home/username/Scratch/myR_job.R > myR_job.out

# Preferably, tar-up (archive) all output files to transfer them back 
# to your space. This will include the R_output file above.
tar zcvf $HOME/Scratch/R_output/files_from_job_$JOB_ID.tgz $TMPDIR

# Make sure you have given enough time for the copy to complete!
```

You will need to change the `-wd /home/<your_UCL_id>/Scratch/R_output` location and the location of your R input file, called `myR_job.R` here.  `myR_job.out` is the file we are redirecting the output into. The output file is saved in the tar archive produced by the last command in the runscript and will be in `$HOME/Scratch/R_output`.

If your jobscript is called `run-R.sh` then your job submission command would be:
```
qsub run-R.sh
``` 

## Example multi-node parallel job using Rmpi and doMPI

This script uses Rmpi and doMPI to allow it to run across multiple nodes using MPI.

To try this example, save this job script and the following R script (`doMPI_example.R`) in your home directory.

```
#!/bin/bash -l

# Batch script to run an MPI parallel R job using the doMPI package
# with the upgraded software stack under SGE with OpenMPI.

# Request ten minutes of wallclock time (format hours:minutes:seconds).
#$ -l h_rt=0:10:0

# Request 1 gigabyte of RAM per process.
#$ -l mem=1G

# Set tmpfs to 1 gigabyte of TMPDIR space (default is 10 GB)
# Remove this for clusters without temporary filesystems, e.g. Kathleen
#$ -l tmpfs=1G

# Set the name of the job.
#$ -N R-doMPI-example

# Select the MPI parallel environment with 12 processes, the maximum possible
# on Myriad would be 36. On Kathleen, request at least 41 processes.
#$ -pe mpi 12

# Set the working directory to the current directory. In this case, we use the home directory.
#$ -cwd

module -f unload compilers mpi gcc-libs
module load r/r-4.4.2_bc-3.20

# Run our MPI job.  GERun is a wrapper that launches MPI jobs on UCL clusters.

gerun Rscript doMPI_example.R
```
The output is saved in `~/R-doMPI-example.o${JOB_ID}`.

If your jobscript is called `run-R-doMPI.sh` then your job submission command would be:
```
qsub run-R-doMPI.sh
```

### Example R script using Rmpi and doMPI

This R script has been written to use Rmpi and doMPI and can be used with the above jobscript. It is `doMPI_example.R` above.

```
# This example uses one of the historic datasets from the HistData package
# and is from Princeton University

# load the Rmpi, doMPI and HistData packages - already installed on UCL clusters.

library (Rmpi)
library (doMPI)
library (HistData)

# This is Galton's data mapping the height of sons to their fathers
# ~900 rows of 2 columns
data (Galton)

# Set up the cluster

cl <- startMPIcluster ()
registerDoMPI (cl)

# Splitting the Galton data frame (mapped to df) into
# units of 100 rows max.

df <- Galton
n <- 100
nr <- nrow (df)

# uses rep to specify the break points without having to manually call each
split_df <- split (df, rep(1:ceiling (nr/n), each=n, length.out=nr))

# We might want to know the ratio of the parent's height vs the child's

# foreach takes parameters and passes them to the MPI worker processes
# using the dimension of the parameter with the longest length (only one here)
# .combine= specifies a function that will be used to combine the results, i.e.
# cbind, rbind, c, etc.

df$results <- foreach(i=1:length(split_df), .combine='rbind') %dopar% {

    # We take the split lists of data frame, add a column called $ratio
    # and assign the result just as we would with a non-parallelized operation
    
    result <- split_df[[i]]$parent/split_df[[i]]$child
    as.data.frame (result)
    
    # this result gets rbind-ed together as a column on our df.
}

# Take a look at the df we got back that we could continue working on if we wanted to
head (df)

# close the cluster to properly free up the MPI resources so GE can see that
# the job has finished.

closeCluster (cl)
Rmpi::mpi.quit ()
```

This example was based on Princeton's [Using R on the Research Computing Clusters](https://github.com/PrincetonUniversity/HPC_R_Workshop/blob/6ddac56324021277f163789f7f501fa82d92deca/04_doMPI/04_doMPI.R) repository.

## Using your own R packages

If we do not have R packages installed centrally that you wish to use, you can 
install them in your space on the cluster and tell R where to find them.

First you need to tell R where to install your package to and where to look for user-installed packages, using the R library path.

### Set your R library path

There are several ways to modify your R library path so you can pick up packages that you have installed in your own space.

The easiest way is to add them to the `R_LIBS` environment variable (insert the correct path):
```
export R_LIBS=/your/local/R/library/path:$R_LIBS
```

This is a colon-separated list of directories that R will search through. 

Setting that in your terminal will let you install to that path from inside R and 
should also be put in your jobscript (or your `.bashrc`) when you submit a job 
using those libraries. This appends your directory to the existing value of 
`$R_LIBS` rather than overwriting it so the centrally-installed libraries can still be found.

You can also change the library path for a session from within R:
```
.libPaths(c('~/MyRlibs',.libPaths()))
```

This puts your directory at the beginning of R's search path, and means that `install.packages()` will automatically put packages there and the `library()` function will find libraries in your local directory.

### Install an R package

To install, after setting your library path:

From inside R, you can do
```
install.packages('package_name', repos="http://cran.r-project.org")
```

Or if you have downloaded the tar file, you can do
```
R CMD INSTALL -l /home/username/your_R_libs_directory package.tar.gz
```

If you want to keep some libraries separate, you can have multiple colon-separated paths in your `$R_LIBS` and specify which one you want to install into with `R CMD INSTALL`.

## BioConductor

If you are installing extra packages for BioConductor, check that you are using the same version that the R module you have loaded is using.

Eg. you can find the [BioConductor 3.15 package downloads here](http://www.bioconductor.org/packages/3.15/BiocViews.html#___Software).
