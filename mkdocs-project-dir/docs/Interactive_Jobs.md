---
title: Interactive Job Sessions
categories:
 - User Guide
layout: docs
---

# Interactive Job Sessions

For an interactive session, in slurm we have two ways to do it. You can reserve some compute nodes via the
scheduler with `salloc` and then are logged in live, just like on the login nodes. This is very similar to the `qrsh` of SGE.
These can be used when you want to allocate resources without running a job immediately but also for live visualisation, 
software debugging, or to work up a script to run your program without having to submit each attempt separately to the queue
and wait for it to complete.

On the other hand, you can also immediately launch a job as he same time you allocate for resources with the `srun` command. 
`srun` is a complex command. Without any prior allocation of recources, `srun` requests resources and runs the job (like a 
one-off allocation + run). But if you have previously requested resources, with `salloc` or `sbatch`, then `srun` launches 
tasks *within* that allocation.

```
  Rule of thumb:
  - If you just want to run something immediately (interactive or batch) → use srun.
  - If you want to hold resources for a while and maybe run multiple jobs inside them → use salloc, then run srun inside.
```  
We detail the use of both commands bellow. 

## Requesting access with `salloc`

When using `salloc`, you will be granted a "job allocation", a node with the resources you asked for. This process an take 
a while depending on the amount of resources allocated and the availability of the cluster at the time.
The process to request resources typically takes the form:

```
salloc <options> <command> <arguments to <command>>
```

For example: 
```
salloc -n 8 --mem-per-cpu=512M --time=2:00:00 
```
In this example you are asking to run eight parallel processes within an MPI environment, 512MB RAM per process, for a period of two
hours. This means you are requesting 8 entire nodes. If you do not specify the amount of nodes you need, the scheduler assumes `-n 1`.
If you need to request a specific number of cores to use, you can use the `--cpus-per-task=X` argument. Following the previous example, 
to request only 24 cores: 

```
 salloc -n 1 --cpus-per-task=24  --mem-per-cpu=512M --time=2:00:00
```

If you do not specify the amount of cores, Slurm considers `--cpus-per-task=1` as the default
You can log in into the node running an interactive session with the `srun command`:

```
srun --pty --jobid=<job_allocation> /bin/bash
```

The session will not be automatically closed after closing/exiting the bash session. `scancel` ends interactive shell and release the resources.

```
scancel <job_allocation>
```

**Remember that in kathleen you need to ask for a minimum of 2 nodes while in Myriad you should ask for less than a node (36 cores maximum). If you request the wrong amount of recources for each system It will allow you to allocate them, but you will not be able to run anything successfully**

## Running interactive jobs with `srun`

You can also request resources and immediately run an interactive job using `srun`. The process typically takes the form `srun --pty` → interactive job allocation.

```
 srun -n 8 --mem-per-cpu=512M --time=02:00:00 <your_job>
```
Similarly as before, this example is asking to run eight parallel processes within an MPI environment, 512MB RAM per process, for a period of two
hours and at the same time, ejecute immediately <your_job>.

You can login into the node where your interactive job is running the with the same `srun` command stated before.

```
srun --pty --jobid=<job_allocation> /bin/bash
```

All job types we support on the system are supported via an interactive session (see our [examples section](Example_Jobscripts.md)).


In SGE the `-now` option was useful when a cluster was busy.  By default qrsh and qlogin jobs will run on the next scheduling cycle or give up. The `-now no` option tells it to keep waiting until it gets scheduled. This option is not present in Slurm as Slurm jobs wait in queue by default, unless you specifically request `--immediate`, which is rarely used.

More resources can be found here:

* [Moodle](https://moodle.ucl.ac.uk/mod/page/view.php?id=4846689) (UCL users)
* [Mediacentral](https://mediacentral.ucl.ac.uk/Play/98393) (non-UCL users)


## Interactive X sessions

You can get an interactive X session (If SLURM supports X11 forwarding (--x11)) ??? 
in two ways: 

```
salloc --x11 -n 1 --mem=2G --time=2:00:00
```
This allocates a node with X11 forwarding enabled. Then run your GUI program:
```
srun --x11 <command>
```
Your GUI will display back on your local machine. Where `<command>` is either a command to launch an X terminal like
Xterm or Mrxvt or a GUI application like XMGrace or GaussView.

Another way to do this is by requesting the resources normally

```
salloc -n 1 --mem=2G --time=2:00:00
```
and then run
```
srun -pty xterm
```
To make effective use of the X forwarding you will need to have logged in to the login node with ssh -X or some equivalent method. 

## Working on the nodes

 If you want to run a command on one of your other allocated nodes, you
can use a standard `ssh` command from the interactive session: 
```
ssh <hostname> <command> [args]
```
to access other nodes within your allocation. Note that you are not
able to `ssh` directly from the login node.

In the above, `<hostname>` can be obtained by inspecting the file
`$TMPDIR/machines`.

## GPU test nodes

You can also run GPU jobs interactively simply by adding the `-l gpu=1`
or `-l gpu=2` options to the qrsh command as normal.

For more information, please contact us on <rc-support@ucl.ac.uk>

