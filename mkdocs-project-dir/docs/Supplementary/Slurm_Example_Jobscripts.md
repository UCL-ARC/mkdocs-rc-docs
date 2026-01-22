# Slurm Example Jobscripts

## Multi-node MPI job

In this example all modules are purged and then reloaded. This is not necessary, but since your current environment gets copied to the job, this makes sure that the batch job will still behave in the same way even if you are working with different local modules interactively on the login node you are submitting from. 

```
#!/bin/bash

#SBATCH --job-name=vasp_6.4.3_2node
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=40
#SBATCH --cpus-per-task=1
#SBATCH --time=4:00:00

# Purging and reloading all modules to be sure of the job's enviroment
module purge
module load ucl-stack/2025-05
module load mpi/intel-oneapi-mpi/2021.14.0/intel-oneapi-2024.2.1
module load intel-oneapi-mkl/2023.2.0-intel-oneapi-mpi/intel-oneapi-2024.2.1
module load hdf5/1.14.3-intel-oneapi-mpi/oneapi-2024.2.1
module load vasp/6.4.3-intel-oneapi-mpi/oneapi-2024.2.1

srun vasp_std
```

## Hybrid MPI with OpenMP job

Here are two example NAMD jobs that use some MPI processes which can communicate between nodes, and some OpenMP threads inside a node.

NAMD uses charm++ which can be complex to launch.

```
#!/bin/bash
# NAMD build using verbs, has more complex launch line.

# Based on ARCHER2 mpi+smp jobscript
# 2 nodes, 2 MPI comm processes per node, 20 worker threads per comm process
#SBATCH --job-name=namd_verbs_2node
#SBATCH --ntasks-per-node=2
#SBATCH --cpus-per-task=20
#SBATCH --nodes=2
#SBATCH --time=4:00:00

# Purging and reloading all modules to be sure of the job's enviroment
module purge
module load ucl-stack/2025-05
module load namd/3.0.1-verbs/gcc-12.3.0

# Set procs per node (PPN) & OMP_NUM_THREADS
# One CPU core left free for associated MPI process
export PPN=$(($SLURM_CPUS_PER_TASK-1))
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
export OMP_PLACES=cores

# Record PPN in the output file
echo "Number of worker threads PPN = $PPN"

# Cannot use srun directly with verbs version as charmrun is required 
# so Slurm options need to be passed as environment variables.
# ++verbose is being passed for more debug information on what charmrun is doing.
export SLURM_HINT=nomultithread
charmrun ++n ${SLURM_NTASKS} ++mpiexec ++remote-shell srun ++verbose $(which namd3) +setcpuaffinity ++ppn ${PPN} apoa1.namd
```

```
#!/bin/bash
# NAMD build using ucx, simpler launch line, may have more overheads.

# Based on ARCHER2 mpi+smp jobscript
# 2 nodes, 2 MPI comm processes per node, 20 worker threads per comm process
#SBATCH --job-name=namd_verbs_2node
#SBATCH --ntasks-per-node=2
#SBATCH --cpus-per-task=20
#SBATCH --nodes=2
#SBATCH --time=4:00:00

# Purging and reloading all modules to be sure of the job's enviroment
module purge
module load ucl-stack/2025-05
module load namd/3.0.1-ucx/gcc-12.3.0

# Set procs per node (PPN) & OMP_NUM_THREADS
# One CPU core left free for associated MPI process
export PPN=$(($SLURM_CPUS_PER_TASK-1))
export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
export OMP_PLACES=cores

# Record PPN in the output file
echo "Number of worker threads PPN = $PPN"

# Setting nomultithread can be omitted as set by default.
srun --hint=nomultithread namd3 +setcpuaffinity +ppn $PPN apoa1.namd
```

## Array job

A job array is a group of jobs with the same executable and resource requirements, but different input files. A job array can be submitted, controlled, and monitored as a single unit. Each job submitted from a job array shares the same job ID as the job array and has its own `$SLURM_ARRAY_TASK_ID`. 

At present, the maximum allowed size of a job array is 1000 tasks.

```
#!/bin/bash
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --job-name=myArray
#SBATCH --time=00:30:00
#SBATCH --array=1-10

# Each task processes a different csv data file
myprogram dataset${SLURM_ARRAY_TASK_ID}.csv
```

The array is created by Slurm directive `--array=1-10` by including elements numbered [1-10] to represent our 10 input files.

When the array job is submitted, Slurm will create 10 jobs under the single job ID. The job array script is submitted in the usual way:

```
sbatch myarrayjob
```

## Enabling Hyperthreads

Hyperthreading lets you use two virtual cores instead of one physical core - some programs can take advantage of this.

On clusters with hyperthreading enabled (Kathleen, Young, Michael), you can request it with:

```
#SBATCH --hint=multithread
```

This tells Slurm to schedule two threads per physical core.

By default we have set `--hint=nomultithread` so hyperthreads will not be used unless requested.

## Examples for Young

### CPU only jobs

#### Single node script
```
#!/bin/bash

#SBATCH --nodes=1
#SBATCH --ntasks-per-node=8
#SBATCH --mem-per-cpu=4G
#SBATCH --time=12:00:00
#SBATCH --job-name=CPU_Single_Node

# Purging and reloading all modules to be sure of the job's environment
module purge
module load ucl-stack/2025-12

# Loading the modules your application needs to execute
module load <name of the module(s) you need>

# Lines to execute below here
<application execution here>

```

#### Single node interactive session
The resource request is identical to the script above. However, once the resources are allocated you'll be migrated to the compute node and will need to then execute any lines manually.  The module commands and `<application execution here>` would be done by-hand.  You'll notice also the `--pty` flag at the end which stands for "pseudo terminal" and is letting Slurm know to start the session using `bash`.

```
srun --nodes=1 --ntasks-per-node=8 --mem-per-cpu=4G --time=12:00:00 --pty bash -l
```

#### Multi-node script

By default the `mpi/openmpi/4.1.6/gcc-12.3.0-avx2` module is loaded.  If you need a cuda enabled MPI, Intel's MPI, or another specific version you can check what is available with the `module avail mpi` command.  If you see any with a seemingly random hash after the name, those are installed as dependencies for other modules.  The ones with `-avx2` or with no hash are the modules you generally should use.

```
#!/bin/bash

#SBATCH --nodes=4
#SBATCH --ntasks-per-node=16
#SBATCH --mem-per-cpu=2G
#SBATCH --time=12:00:00
#SBATCH --job-name=CPU_Multi_Node

# Purging and reloading to be sure of the job's environment
module purge
module load ucl-stack/2025-12

# Loading the MPI you'll use for execution.  Here we're going with the default openmpi.
module load mpi/openmpi/4.1.6/gcc-12.3.0-avx2

# Loading the modules your application needs to execute
module load <name of the module(s) you need

# Lines to execute below here
srun <application to execute>

```

#### Multi-node interactive session
`salloc --nodes=4 --ntasks-per-node=16 --mem-per-cpu=2G --time=12:00:00`

This will start a new session on the login node with the requested resources allocated.  As with the `srun` example above you'll now need to load the correct modules.  This time however you'll execute your command with `srun` as you did in the multi-node script.  `srun` will detect you're in a session and have resources allocated and will then execute your command using the loaded MPI, in-parallel.

### High-Bandwidth (HBM) memory jobs

#### Single node HBM script
```
#!/bin/bash

#SBATCH --parition=hbm
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=4
#SBATCH --mem-per-cpu=64G
#SBATCH --gres=tmpfs:40G
#SBATCH --time=12:00:00
#SBATCH --job-name=HBM_Job

# Purging and reloading all modules to be sure of the job's environment
module purge
module load ucl-stack/2025-12

# Loading the modules your application needs to execute
module load <name of the module(s) you need

# Printing the location of the temporary storage
echo $TMPDIR

# Lines to execute below here
<application execution here>

```

Note that the `hbm` partition you need to explicitly specify.  If you don't, the job will run on a regular CPU node.  In this example we've also asked for 40G of temporary storage on the node.  

#### Single node HBM interactive session
`srun --parition=hbm --nodes=1 --ntasks-per-node=4 --mem-per-cpu=64G --gres=tmpfs:40G --time=12:00:00 --pty bash -l`

### GPU jobs

#### GPU node script
```
#!/bin/bash

#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --mem-per-cpu=8G
#SBATCH --gres=gpu:1,tmpfs:20G
#SBATCH --time=12:00:00
#SBATCH --job-name=GPU_Single_GPU

# Purging and reloading all modules to be sure of the job's environment
module purge
module load ucl-stack/2025-12

# Loading the modules your application needs to execute
module load <name of the module(s) you need

# Outputting the nvidia information about the node
nvidia-smi

# Printing the location of the temporary directory
echo $TMPDIR

# Lines to execute below here
<application execution here>

```

Note that we're requesting both one GPU and 20G of temporary storage.

#### GPU node interactive session
`srun --nodes=1 --ntasks-per-node=1 --mem-per-cpu=8G --gres=gpu:1,tmpfs:20G --time=12:00:00 --pty bash -l`

### What is the deal with srun and salloc?

If you're confused about why `srun` is used above to start an interactive session but also used to execute a parallel application within a script, there is no need to worry -- it is confusing!  `srun` is a very versitile command.  When used with no inputs it will execute whatever comes after it on all resources available.  If it detects you're using MPI it will execute in parallel using MPI.  If there is no MPI it will execute the command once on each of the available allocated CPUs.  If there are no resources yet allocated via `sbatch` or `salloc` it will look to see if you've asked for resources, allocate them for you, and then run the command.  If you feed it the `--pty` you can NOT specify a command and it will start an interactive session for you.  Versitile!

The more straight-forward interactive session command is `salloc`.  There is no need to specify a shell as it starts a session for you, you don't have to worry about the environment on a compute node being different to the login node as your new session stays on the login node, and you can use srun to initiate parallel job.  The two methods are very similar, just remember that when you use `salloc` to start an interactive session you need to use `srun <application executable>` to run on the allocated compute resources, as you would in a submission script.

