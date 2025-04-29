---
title: Data Storage
categories:
 - User Guide
 - Background
layout: docs
---

# Data Storage

Our clusters have local [parallel filesystems](Parallel_Filesystems.md) consisting of 
your home and Scratch directories where you can write data. These are "close to" the 
compute, connected to it with a fast network.

They may also have local storage on the compute node that can be used during your job.

## On-cluster storage

### Home

Every user has a home directory. This is the directory you are in when you first log in.

- Location: `/home/<username>`
- May also be referred to as: `~`, `$HOME`.

Many programs will write hidden config files in here, with names beginning with `.` (eg `.config`,
`.cache`). You can see these with `ls -al`.

### Scratch

Every user also has a Scratch directory. On Myriad and Kathleen this exists for backwards 
compatibility and all of your home space should be considered scratch.

It is intended that this is a larger space to keep your working files as you do your 
research. It should not be relied on for secure long-term permanent storage.

Important data should be regularly backed up to another location.

- Location: `/home/<username>/Scratch` (either a directory or a shortcut or symbolic link to a different location).

### Temporary storage for jobs (TMPDIR)

If the cluster you are on is not described in its about page as being diskless, the compute nodes
will have local disks that can be written to during your job. 

This is because a local disk is faster to write to than a parallel filesystem. 

This location is created on the compute node when your job begins, and deleted again when it ends.
You will need to copy the data back out of it before the end of your job into your Scratch. If your
job fails or runs out of time, you will not be able to recover this data.

- Location: `$TMPDIR`
- Will only exist during your job and be deleted after.


## ARC Cluster File System (ACFS)

The ARC Cluster File System (ACFS) is ARC's centralised storage system that will be available from
multiple ARC systems.

It is the backed-up location for data which you wish to keep.

The ACFS is available read-write on the login nodes but read-only on the compute nodes. This means
that your jobs can read from it, but not write to it, and it is intended that you copy data onto
it after deciding which outputs from your jobs are important to keep.

Available on Myriad and Kathleen, you will be able to see the same files from both clusters.

- Location: `/acfs/users/<username>`
- Also at: `/home/<username>/ACFS` (a shortcut or symbolic link to the first location).
- Backed up daily.
