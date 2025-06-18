# Slurm

We have changed schedulers from SGE to Slurm. Both are job schedulers for HPC clusters - they have different architecture, commands, and features. 

Here is a comparative table of the main SGE commands and their equivalent in Slurm. (Since we had a very custom SGE install, a few commands may be slightly different or may not have been available in our past setup).

<figure id="rosetta">
<div class="center">
<img src="../../img/Rosetta_Slurm-SGE.png" style="width:150mm" alt="SGE and Slurm command Rosetta stone" />
</div>
<figcaption><em> <span id="rosetta" label="rosetta">SGE and Slurm command Rosetta stone</span></em></figcaption>
</figure>

[Full Slurm Rosetta stone of Workload Managers available as PDF](https://slurm.schedmd.com/rosetta.html)

## How do I submit a job to Slurm?

You can submit a bash jobscript that has Slurm directives in it. You can also pass in Slurm directives on the command line to the Slurm submit commands.

### Using a bash jobscript

To submit a job to the scheduler you need to write a jobscript that contains the resources the job is asking for and the actual commands/programs you want to run. This jobscript is then submitted using the `sbatch` command:

```
sbatch myjobscript
```

Lines beginning with `#SBATCH` in your jobscript contain options to `sbatch`. Slurm will take each line starting with `#SBATCH` and use the contents beyond that as an instruction. The job will be put in to the queue and will begin running on the compute nodes at some point later when it has been allocated resources.

Slurm allows you to specify various options for how your job is laid out across nodes.

```
#!/bin/bash

# Request two nodes on Kathleen and run 40 tasks per node, one cpu each:
#SBATCH --nodes=2
#SBATCH --ntasks-per-node=40
#SBATCH --cpus-per-task=1
#SBATCH --time=00:10:00
```

This will give you the same layout on Kathleen:

```
#!/bin/bash

# Request 80 tasks with one cpu each:
#SBATCH --ntasks=80
#SBATCH --cpus-per-task=1
#SBATCH --time=00:10:00
```

You can launch parallel tasks by using `srun` inside the jobscript you are running with `sbatch`. For most programs, this replaces the `mpirun` or our previous `gerun` command.

```
#!/bin/bash

# Request 80 tasks:
#SBATCH --ntasks=80
#SBATCH --time=00:10:00

srun myprog
```

Practical examples of how to run parallel tasks can be found in [Slurm Example Jobscripts](Slurm_Example_Jobscripts.md).

### Passing in command-line arguments

You can also pass options directly to the `sbatch` command and this will override the settings in your script. This can be useful if you are scripting your job submissions in more complicated ways.

For example, if you want to change the name of the job for this one instance of the job you can submit your script with:

```
sbatch --job-name=NewName myscript.sh
```

Or if you want to alter the wallclock time in the existing script to 24 hours:

```
sbatch -t 0-24:00:00 myscript.sh
```

You can also use `srun` to submit your jobscript. `srun` only accepts command-line arguments.

```
srun --ntasks=80 --time=00:10:00 myjobscript
```

You can submit jobs with dependencies by using the `--depend` option. For example, the command below submits a job that won't run until job 12345 has finished:

```
sbatch --depend=12345 myscript.sh
```

Note that for future reference, it helps if you have these options inside your jobscript rather than passed in on the command line whenever possible, so there is one place to check what your past jobs were requesting.

### Checking your previous jobscripts

If you want to check what you submitted for a specific job ID, you can still do this with the `scriptfor` utility. (This now runs the `sacct` command for you with relevant options).

```
scriptfor 12345
```

As mentioned above, this will not show any command line options you passed in.

### Checking your whole submit-time environment

By default, jobs will copy your current environment that you have on the login node you are submitting from (`--export=ALL` from the command comparison table). This is different from our previous setup. You can view the whole environment that your job was submitted with using the `envfor` utility. (This also runs `sacct` with relevant options).

```
envfor 12345
```

This output can be long and contain multi-line shell functions.

## How do I monitor a job?

### squeue

The `squeue --me` command shows the status of your jobs. By default, if you run it with no options, it shows all jobs. This makes it easier to keep track of your jobs.

The output will look something like this:

```
  JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
     22  kathleen lammps_b  ccxxxxx  R       0:04      2 node-c11b-[002-003]
```

This shows you the job ID, the partition it is using, the first 8 characters from the name you have given the job, your username, the state the job is in, how long it has been waiting (or running, if it has begun), the number of nodes it requested, and what nodes it is running on.

You can get a little more information with the `-l` or `--long` option: 

```
Tue Jun 17 12:15:57 2025
  JOBID PARTITION     NAME     USER    STATE       TIME TIME_LIMI  NODES NODELIST(REASON)
     22  kathleen lammps_b  ccxxxxx  RUNNING       1:16   2:00:00      2 node-c11b-[002-003]
```

`squeue --help` will show how you can format the output differently.

### scontrol

This is a utility to display detailed information about the specified active job.

```
scontrol show job 123454
```

### scancel

You use `scancel` to delete a job from the queue.

```
scancel 123454
```

You can delete all your jobs at once (this acts on your own user):

```
scancel '*'
```

### More scheduler commands

Have a look at `man squeue` and note the commands shown in the SEE ALSO section of the manual page. Exit the manual page and then look at the man pages for those. (You will not be able to run all commands).

### Past jobs

Once a job ends, it no longer shows up in `squeue`. To see information about your finished jobs - when they started, when they ended, what node they ran on - use the command `sacct`.

```
# show my jobs from the last two days
sacct -X -o "jobid,user,jobname,start,end,alloccpus,nodelist,state,exitcode" -S now-2days
```

```
JobID             User    JobName               Start                 End  AllocCPUS        NodeList      State ExitCode 
------------ --------- ---------- ------------------- ------------------- ---------- --------------- ---------- -------- 
19             ccxxxxx lammps_bt+ 2025-06-16T14:13:39 2025-06-16T14:13:44        160 node-c11b-[002+     FAILED      2:0 
22             ccxxxxx lammps_bt+ 2025-06-17T12:14:41             Unknown        160 node-c11b-[002+    RUNNING      0:0
```

If a job ended and didn't create the files you expect, check the start and end times to see whether it ran out of wallclock time.

If a job only ran for seconds and didn't produce the expected output, there was probably something wrong in your script - check the .out.txt and .err.txt files in the directory you submitted the job from for errors.
