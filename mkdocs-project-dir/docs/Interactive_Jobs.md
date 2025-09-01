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
We detail the use of both commands bellow. 

## Requesting Access with `salloc`

You will be granted an interactive shell after running a command that
checks with the scheduler whether the resources you wish to use in your
tests/analysis are available. Interactive sessions are requested using the salloc command. 
It typically takes the form:

```
srun --pty -n 8 --mem-per-cpu=512M --time=2:00:00 bash
```

In this example you are asking to run eight parallel processes
within an MPI environment, 512MB RAM per process, for a period of two
hours.


srun --pty → interactive job allocation

-n 8 → request 8 tasks (like 8 MPI ranks)

--mem-per-cpu=512M → 512 MB memory per task (matches SGE mem=512M)

--time=2:00:00 → 2 hours runtime limit

bash → starts an interactive shell

If you wanted to queue the job (like -now no), that’s the default in SLURM—jobs wait in queue unless you specifically request --immediate, which is rarely used.


## Running interactive jobs with `srun`

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

```
qrsh <options> <command> <arguments to <command>>
```

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

