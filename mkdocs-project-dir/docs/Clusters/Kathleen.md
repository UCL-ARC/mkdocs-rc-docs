---
title: Kathleen
layout: cluster
categories: 
 - missing-info
 - needs-update
---
# Kathleen

Kathleen is a compute cluster designed for extensively parallel, multi-node batch-processing jobs, having high-bandwidth connections between each individual node. It is named after [Professor Dame Kathleen Lonsdale](https://en.wikipedia.org/wiki/Kathleen_Lonsdale), a pioneering chemist and activist, and was installed in December 2019. It went into service at the end of January 2020.

## New operating system and scheduler

Kathleen is currently updated to our new operating system (RHEL9) and new scheduler, Slurm.

It can be accessed via `kathleen-ng.rc.ucl.ac.uk` which will log you in to updated login nodes.

The new software stack is provided by a module called `ucl-stack/2026-03` which should be being loaded by default. If not (for example if you `module purge` in your .bashrc) you can load the new environment by:

```
module load ucl-stack/2026-03
module load default-modules/2026-03
```

The default compiler is now GCC and the default MPI is OpenMPI. We do also have Intel OneAPI modules.

Your jobscripts will need to be rewritten as Slurm jobscripts, and there are different commands for submitting jobs and checking your job status.

* [Usage of Slurm](../Supplementary/Slurm.md)
* [Slurm Example Jobscripts](../Supplementary/Slurm_Example_Jobscripts.md)

## Accounts

Kathleen accounts can be applied for via the [Research Computing sign up process](../Account_Services.md).

As Kathleen is intended for multi-node jobs, users who specify that they will need to use multi-node jobs (e.g. with [MPI](../Supplementary/Glossary.md#mpi)) will be given access to Kathleen.

## Logging in

Please use your UCL username and password to connect to Kathleen with an SSH client.

```
# Kathleen, RHEL9 and Slurm
ssh uccaxxx@kathleen-ng.rc.ucl.ac.uk
```

If using PuTTY, put `kathleen-ng.rc.ucl.ac.uk` as the hostname and your
seven-character username (with no @ after) as the username when logging
in, eg. `uccaxxx`. When entering your password in PuTTY no characters or
bulletpoints will show on screen - this is normal.

If you are outside the UCL firewall you will need to follow the
instructions for [Logging in from outside the UCL firewall](../howto.md#logging-in-from-outside-the-ucl-firewall).

The login nodes allow you to manage your files, compile code and submit jobs. Very short (\<15mins) and non-resource-intensive software tests can be run on the login nodes, but anything more should be submitted as a job.

### Logging in to a specific node

You can access a specific Kathleen login node by using their dedicated addresses instead of the main `kathleen-ng.rc.ucl.ac.uk` address, for example:

```
ssh uccaxxx@login21.kathleen.rc.ucl.ac.uk
```

The main address will unpredictably direct you to either one of these (to balance load), so if you need multiple sessions on one, this lets you do that.

## Copying data onto Kathleen

You will need to use an SCP or SFTP client to copy data onto Kathleen.
Please refer to the page on [How do I transfer data onto the system?](../howto.md#how-do-i-transfer-data-onto-the-system)

If you find you cannot connect directly from one cluster to another, this is 
generally because of firewalls in between and so you need to use [tunnelling with the scp command](../howto.md#single-step-logins-using-tunnelling).

## Quotas

On Kathleen you have a single 250GB quota by default which covers your home and Scratch. 

This is a hard quota: once you reach it, you will no longer be able to write more data. Keep an eye on it, as this will cause jobs to fail if they cannot create their .o or .e files at the start, or their output files partway through.

You can check your quota on Kathleen by running:

```
lquota
```

which will give you output similar to this:

```
     Storage        Used        Quota   % Used   Path
      lustre  146.19 MiB   250.00 GiB       0%   /home/uccaxxx
```

You can apply for quota increases using the form at [Additional Resource Requests](../Additional_Resource_Requests.md).

Here are some tips for [managing your quota](../howto.md#managing-your-quota) and
finding where space is being used.

## Job sizes and durations

Please consider that Kathleen nodes have 40 physical cores - 2 nodes is 80 cores. Jobs do not share nodes, so although asking for 41 cores is possible, it means you are wasting the other 39 cores on your second node!

For interactive and batch jobs:

| Cores    | Max. Duration |
|:--------:|:-------------:|
| 41-240   | 48h           |
| 241-480  | 24h           |
| 481-5760 | 12h           |

These are numbers of physical cores.

If you have a workload that requires longer jobs than this, you may be able to apply to our governance group for access to a longer queue. Applications will be expected to demonstrate that their work cannot be run using techniques like checkpointing that would allow their workload to be broken up into smaller parts. Please see the section on [Additional Resource Requests](../Additional_Resource_Requests.md) for more details.

The memory you request is always per core, not the total amount. Each node has 192 gigabytes of RAM, and each node has 40 cores, e.g. a job requesting for 40 cores should ask for no more than 4.8G RAM (192/40).

## Node types

Kathleen's compute capability comprises 192 diskless compute nodes each with two 20-core [Intel Xeon Gold 6248 2.5GHz](https://ark.intel.com/content/www/us/en/ark/products/192446/intel-xeon-gold-6248-processor-27-5m-cache-2-50-ghz.html) processors, 192 gigabytes of 2933MHz DDR4 RAM, and an Intel OmniPath network.

Two nodes identical to these, but with two 1 terabyte hard-disk drives added, serve as the login nodes.

## Hyperthreading

On clusters with hyperthreading enabled (Kathleen, Young, Michael), you can request it with:

```
#SBATCH --hint=multithread
```

This tells Slurm to schedule two threads per physical core.

By default we have set `--hint=nomultithread` so hyperthreads will not be used unless requested.

Hyperthreading lets you use two virtual cores instead of one physical core - some programs can take advantage of this. 

If you do not ask for hyperthreading, your job only uses one thread per core as normal.


## Diskless nodes

Kathleen nodes are diskless (have no local hard drives) - there is no `$TMPDIR` available on Kathleen, so you should not request `-l tmpfs=10G` in your jobscripts or your job will be rejected at submit time.

If you need temporary space, you should use somewhere in your Scratch.


## New software stack

Not everything contained in the stack is visible by default - we have made the applications that we expect people 
to use directly visible and lots of their dependencies are hidden. These will show up if you search for that package 
specifically, for example:

```
module avail libpng
-------------------------- /shared/ucl/apps/spack/0.23/deploy/2025-02/modules/applications/linux-rhel7-cascadelake --------------------------
libpng/1.6.39/gcc-12.3.0-iopfrab  
```

This module does not show up in the full list but is still installed. It has a hash at the end of its name `-iopfrab`
and this will change over time with different builds.

If you find you are needing one of these modules often, let us know and we'll make it one that is not hidden in the 
next release of this stack.

If you'd like to view all available modules, the ones we've chosen to be visible and all dependant packages, you can use the `--all` flag:

```
module avail --all
```
