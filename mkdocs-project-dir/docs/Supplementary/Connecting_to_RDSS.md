---
title: Connecting to the Research Data Storage Service
categories:
 - Myriad
layout: docs
---
The [Research Data Storage Service](https://www.ucl.ac.uk/advanced-research-computing/platforms-and-services/research-data-storage-service)
(RDSS) is a system run by the Research Data group in the Advanced Research
Computing department, and is designed to help with data storage
during and after a project. Several solutions for copying data between
RDSS and the central UCL research computing platforms are presented below.
Sections of the example code surrounded by angle brackets (\<\>) should 
be replaced by the information indicated (do not keep the angle brackets in).

## Between Myriad or Kathleen and RDSS

If you already have an account with the Research Data Storage Service, you can
transfer data directly between Myriad or Kathleen and Research Data Storage using
the Secure Copy (`scp`) command, copy (`cp`) command or `rsync` command.

### From RDS to Myriad

If you are on an RDSS login node, you can transfer data to
Myriad’s Scratch area at the highest rate currently possible by running
the command: 

```
scp data_file.tgz myriad.rc.ucl.ac.uk:~/Scratch/
```

Or from somewhere within Myriad (including compute nodes in
running jobs) running the command: 

```
scp ssh.rd.ucl.ac.uk:~/data_file.tgz ~/Scratch/
```

### From Myriad or Kathleen to RDSS

The RDSS is mounted on the Myriad and Kathleen login nodes at `/rdss`. There are 
subdirectories `rd00`, `rd01` that contain projects with names beginning 
`ritd-ag-project-rd00` or `ritd-ag-project-rd01` and so on. You can use `cp` or local
`rsync` commands to copy data between your RDSS space and your Myriad space.

Rsync is useful as rerunning it will not copy files that already exist in the 
destination - so it can be used to synchronise data and to continue if a transfer was 
interrupted partway through.

```
# Use rsync archive mode (recursively copy directories, copy symlinks without resolving,
# and preserve permissions, ownership and modification times) to copy the data directory
# and everything in it from RDSS to Scratch.
# --safe-links ignores any symbolic links that point outside the copied tree and
# any symlinks that are absolute paths
rsync --safe-links -r -a /rdss/rd00/ritd-ag-project-rd001/ritd-ag-project-rd0001-test/data ~/Scratch
```

You can use the copy command. It has fewer options than rsync and is more suited for simpler
and smaller copies.

```
# Copy the data directory from RDSS to Scratch recursively and preserve permissions.
cp -rp /rdss/rd00/ritd-ag-project-rd001/ritd-ag-project-rd0001-test/data ~/Scratch
```

You can still use scp to do a remote copy to the RDSS ssh node and send data to your project 
space on RDSS by running the command:

```
scp data_file.tgz ccaaxyz@ssh.rd.ucl.ac.uk:<path_to_project_space>
``` 

Note that the old Myriad transfer node `transfer02` is no longer accessible now that the login nodes
can be used instead.

The RDSS support pages provide more
information:

<https://www.ucl.ac.uk/advanced-research-computing/rdss-myriad-data-storage-transfer-service>

