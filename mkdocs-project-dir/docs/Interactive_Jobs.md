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

On the other hand, you can also immediately launch a job as he same time you allocate for resources wit the `srun` command. 
Slurm jobs wait in queue by default, unless you specifically request `--immediate`, which is rarely used.
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
You can login into the node where your interactive job is running the with the same `srun` command stated before.

```
srun --pty --jobid=<job_allocation> /bin/bash
```


All job types we support on the system are supported via an interactive
session (see our [examples section](Example_Jobscripts.md)).
Likewise, all qsub options are supported like regular job submission
with the difference that with qrsh they must be given at the command
line, and not with any job script (or via -@).

In addition the `-now` option is useful when a cluster is busy. 
By default qrsh and qlogin jobs will run on the next scheduling
cycle or give up. The `-now no` option tells it to keep waiting
until it gets scheduled. Pressing Ctrl+C (i.e. the control key
and the C key at the same time) will safely cancel the request
if it doesn't seem to be able to get you a session.

More resources can be found here:

* [Moodle](https://moodle.ucl.ac.uk/mod/page/view.php?id=4846689) (UCL users)
* [Mediacentral](https://mediacentral.ucl.ac.uk/Play/98393) (non-UCL users)


srun
```
srun --pty -n 8 --mem-per-cpu=512M --time=2:00:00 bash
```
Purpose: Actually launch tasks (your program) on allocated resources.

Can be used in two ways:

Without prior allocation: srun requests resources and runs the job (like a one-off allocation + run).

Inside an allocation (salloc or sbatch): srun launches tasks within that allocation.

Example direct execution:


Log in into the node running an interactive node 
srun --pty --jobid=… bash


## Interactive X sessions

You can get an interactive X session from the head node of the job back
to the login node. The way to do this is to run the `qrsh` command in the
following generic fashion:

`

Where `<command>` is either a command to launch an X terminal like
Xterm or Mrxvt or a GUI application like XMGrace or GaussView.

To make effective use of the X forwarding you will need to have logged
in to the login node with ssh -X or some equivalent method. Here is an
example of how you can get a X terminal session with the qrsh command:

```
qrsh -l mem=512M,h_rt=0:30:0 \
   "/shared/ucl/apps/mrxvt/0.5.4/bin/mrxvt -title 'User Test Node'"
``

***NOTE: If we run salloc --cpus-per-task=40 --time=00:10:00, it will execute fine but if we adk more than --cpus-per-task=40, it says that node configuration is not available? Why is that? It is supossed Kathleen is for multinode purpose...***


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

