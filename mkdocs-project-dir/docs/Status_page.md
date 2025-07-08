---
layout: docs
---

# Status of machines

This page outlines that status of each of the machines managed by the Research Computing team at UCL. We endeavour to keep this page as up-to-date as possible but there might be some delay. Also there are spontaneous errors that we have to deal with (i.e. high load on login nodes) but feel free to report them to rc-support@ucl.ac.uk. Finally, details of our planned outages can be found [here](https://www.rc.ucl.ac.uk/docs/Planned_Outages/).  

### Myriad

  - 2023-03-06 - Myriad's filesystem is getting full again, which will impact performance. If you are 
    able, please consider backing up and deleting any files that you aren't actively using for your 
    research for the time being.
    
    You can check your quota and see how much space you are using on Myriad with the `lquota` command, 
    and you can see which directories are taking the most space using the `du` command, which can also 
    be run in specific directories. You can tell `du` to only output details of the first level of 
    directory sizes with the `--max-depth=1` option.
    
    Jobs are still running for the moment, but if the filesystem keeps getting fuller we may need to 
    stop new jobs running until usage is brought down again.

  - 2023-07-31 12:30 - Myriad's filesystem is currently being very slow because we have two failed 
    drives in the Object Store Target storage. One drive has fully failed and the volume is under 
    reconstruction. The other has been detected as failing soon and is being copied to a spare, but
    this is happening slowly, likely because the disk is failing.

    They are in separate volumes so there isn't a risk there, but it can take some time for 
    reconstruction to complete and this will make the filesystem sluggish. This will affect you 
    accessing files on the login nodes, and also your jobs reading or writing to files on Scratch 
    or home, and accessing our centrally-installed software. (It won't affect writing to `$TMPDIR`,
    the local hard disk on the compute node).

    Once the reconstruction is complete, performance should return to normal. This could take most 
    of the day and potentially continue into tomorrow.

  - 2023-08-01 11:00 - We have had a report of another impending disk failure in the same volume as
    the first failed disk, which puts data at risk. The volume reconstruction is still in progress 
    and expected to take another 30-odd hours. (The second copy to spare has completed).

    We have stopped Myriad from running jobs to reduce load on the filesystem while the 
    reconstruction completes, so no new jobs will start for the time being. We'll keep you updated 
    if anything changes.

  - 2023-08-03 10:30 - We have another estimated 18 hours of volume reconstruction to go, so we are
    leaving jobs off today. On Friday morning we will check on the status of everything and 
    hopefully be able to re-enable jobs then if all is well.

  - 2023-08-04 10:00 - The reconstruction is complete and we have re-enabled jobs on Myriad.

  - 2023-11-23 14:00 - We need to stop new jobs running on Myriad while its filesystem recovers.

    This means that jobs that are already running will keep running, but new jobs will not start 
    until we enable them again. The filesystem is likely to be slower than usual. You can still log 
    in and access your files.

    This is because we've just had two disks fail in the same area of Myriad's filesystem and the 
    draining of jobs is to reduce usage of the filesystem while it rebuilds onto spare disks.

    The current estimated time for the disk rebuild is 35 hours (but these estimates can be 
    inaccurate).

    We will update you when new jobs can start running again.

  - 2023-11-24 09:45 - **Myriad filesystem is down.** 

    I'm afraid Myriad's filesystem is currently down. You will not be able to log in and jobs are 
    stopped.

    A third disk failed in the same area as the last two and we started getting errors that some 
    areas were unreadable and reconstruction failed. We need to get this area reconstructed so are 
    in contact with our vendors.

    **Detail**

    Our parallel filesystem is Lustre. It has Object Storage Targets that run on the Object 
    Storage Servers. Data gets striped across these so one file will have pieces in multiple 
    different locations, and there is a metadata server that keeps track of what is where. One of 
    the Object Storage Targets has lost three disks, is unreadable and is what needs to be 
    reconstructed. While it is unavailable, the filesystem does not work. We will also have some 
    amount of damaged files which have pieces that are unreadable.

    If the reconstruction succeeds, we will then need to clear out the unreadable sectors. Then we 
    will be able to start checking how much damage there is to the filesystem.

    This is all going to take some time. We will update you again on Monday by midday, but there 
    may be no new information by then.

    I'm sorry about this, we don't know right now how long an interruption this will cause to 
    service, or how much data we may have lost.

    Please send any queries to rc-support@ucl.ac.uk 

  - 2023-11-27 12:10 - There is not much of an update on Myriad's filesystem yet - we've been in 
    contact with our vendors, have done some things they asked us to (reseating hardware) and are 
    waiting for their next update.

    In the meantime, we are starting to copy the backup of home that we have from the backup system
    where it is very slow to access to somewhere faster to access, should we need to recover files 
    from it on to Myriad.

    We received a question about backups - the last successful backup for Myriad home directories 
    ran from Nov 20 23:46 until Nov 22 03:48. Data before that interval should be backed up. Data 
    after that is not. Data created or changed during it may be backed up, it depends on which 
    users it was doing when.

    (Ideally we would be backing up every night, but as you can see the backup takes longer than a 
    day to complete, and then the next one begins. The next backup had started on Nov 22 at 08:00 
    but did not complete before the disk failures).

  - 2023-11-29 11:30 Today's Myriad filesystem update:

    * We are continuing to meet with our vendor and have sent them more data to analyse.

    * We are going to copy over the failed section of filesystem to a new area, with some data 
      loss, so we end up with a working volume that we can use to bring the whole filesystem back 
      up.

    * The copy of data from our backup location to easier access location is ongoing (we have 
      48TiB of home directories consisting of 219M items, currently copied ~~15TiB~~ 7.5TiB and 27M 
      items).

    Date of next update: Monday 4 Dec by midday.

    Myriad will definitely not be back up this week, and likely to not be back up next week - but 
    we'll update you on that. The data copying all takes a lot of time, and once it is there, we 
    need to run filesystem checks on it which also take time.

  - 2023-12-04 12:30 - The data copying is continuing.

    * The copy of the failed section of filesystem to a new area is at 33% done, expected to take 
      4.5 days more.

    * The copy of home directories from our backup location to easier access location is at 
      ~~44TiB~~ 22TiB (GPFS was reporting twice the real disk usage) 
      and 86M items out of 48TiB and 219M. It is hard to predict how long that may take, as the 
      number of items is the bottleneck. It has been going through usernames in alphabetical order 
      and has reached those starting "ucbe".

    If the first of these copies completes successfully, we will then need to go through a 
    filesystem check. This will also take possibly a week or more - it is difficult to predict 
    right now. If this recovery continues as it has been and completes successfully, it looks like 
    we may have lost a relatively small proportion of data - but we cannot guarantee that at this 
    stage, and the copy has not completed.

    So at present we expect to be down at least all of this week, and all of next week - and 
    possibly into the week of the 18th before UCL's Christmas closing on the 22nd.

    I'm sorry about this - with these amounts of data and numbers of files, it gets difficult to 
    predict how long any of these operations will take.

    We've had a couple of questions about whether there is any way people can get their data from 
    Myriad - until the filesystem is reconstructed and brought back up, we cannot see separate 
    files or directories on Myriad.

  - 2023-12-12 15:00 - First data copy complete.

    * The copy of the failed section of filesystem has completed and is showing at 99.92% rescued, 
      leaving around 75GB of data as not recoverable. (At present we don't know what this data is -
      it could be from all of home, Scratch, shared spaces and our application install area).

    * The copy of home directories from our backup location to easier access location is still 
      going, currently at 42.5TiB and 166M items out of 48TiB and 219M. My earlier reports had 
      doubled the amount we had successfully copied, so when I said 44TiB previously, that was only
      22TiB. (Corrected in this page). This is progressing slowly and has stalled a couple of times.

    We next need to bring the filesystem up so we can start running checks and see what files are 
    damaged and missing. We will be discussing our next steps to do this with our vendor. 

    I intend to send another update this week, before the end of Friday. 

  - 2023-12-15 17:20 - No news, Christmas closing.
    
    No news, I'm afraid. It is looking like Myriad's filesystem will definitely remain down over 
    Christmas.

    * Our vendors are investigating filesystem logs.

    * The copy of home directories from our backup location to easier access location appears to 
      have finished the copying stage but was still doing some cleanup earlier today.

    UCL is closed for Christmas from the afternoon of Friday 22 December until 9am on Tuesday 2 
    January. Any tickets received during this time will be looked at on our return.

    We will send an update next week before UCL closes.

  - 2023-12-18 15:30 - Home directories from backup available

    We have restored Myriad HOME directories only (no Scratch, Projects or
    Apps) from the most recent back up which ran from:
 
    Monday Nov 20 23:46 to Wednesday Nov 22 03:48
 
    They are mounted READ ONLY on the Myriad login nodes so you can login
    to check what files are missing or need updating and scp results etc
    back to your local computer. We apologize for the delay in making this
    data available, unfortunately the restore process was only finished during
    the weekend.
 
    Work on restoring more data (i.e. HOME from after the backup, as well
    as Scratch and Projects) is still in progress.
 
    It is currently not possible to run jobs.
 
    We still don't expect Myriad to be restored to service before the
    Christmas and New Year UCL closure.
 
    UCL is closed for Christmas from the afternoon of Friday 22 December until 9am on Tuesday 2 
    January. Any tickets received during this time will be looked at on our return.

  - 2023-12-22 15:00 - A final Myriad update before UCL closes for the Christmas and New Year break.
 
    The copy of rescued data back onto the re-initialised volume completed this morning (Friday 
    22nd). We are now running filesystem checks. Myriad will remain down during the Christmas and 
    New Year closure apart from the read only HOME directories as detailed previously.

  - 2024-01-05 16:50 - An update on the status of Myriad before the weekend

    We wanted to give you a quick update on the progress with Myriad before the weekend as we 
    know several of you are asking for one.
 
    We are meeting on Monday morning to consider options for returning the live filestore 
    including Apps, HOME, Scratch and projects to service. We should have some rough timescale we 
    can give you later on Monday.
 
    Currently a scan is running to discover which files existed either wholly or in part on the 
    failed volume. So far this has discovered around 60M files, and the scan is about halfway. This 
    will carry on running over the weekend. Unfortunately, there is likely to be significant data 
    loss from your Scratch directories.
 
    We will send another update later on Monday.

  - 2024-01-08 17:50 - Myriad update (Monday)
 
    We met this morning to discuss options for returning the live filestore including Apps, HOME, 
    Scratch and projects to service. Tentatively we hope to be able to allow you access to your 
    HOME, Scratch and projects by the end of this week.
 
    The scan to discover which files existed either wholly or in part on the failed volume has 
    completed and found about 70M files which is around 9.1% of the files on Myriad. We are 
    planning to put files in your HOME directory:
 
    One listing the missing files from your HOME directory.
 
    One listing the missing files from your Scratch directory.
 
    There are also missing files in projects so if you own a project we will give you a list of 
    these too.

  - 2024-01-11 12:30 - Jobs on Myriad

    We've had questions from you about when jobs will be able to restart. We were able to assess 
    the damage to our software stack yesterday and most of the centrally installed applications 
    are affected by missing files and need to be copied over from other clusters or reinstalled.

    We're going to begin by copying over what we can from other systems. We'll be looking at the 
    results of this first step and seeing how much is still missing after it and how fundamental 
    those packages are. 

    It would be possible for us to enable jobs before the whole stack was reinstalled, but we need 
    enough there for you to be able to carry out useful work. We should have a much better idea of 
    what is still missing by Monday and our plans for reinstating it. I would rather lean towards 
    giving you access sooner with only the most commonly-used software available rather than 
    waiting for longer.

    We're on schedule for giving you access to your files by the end of this week.

    New user accounts will start being created again once jobs are re-enabled.

    I'll also be sending an update in the next few days about our future filesystem plans and 
    mitigations we were working on before this happened.

#### Check lists of damaged files on Myriad

  - 2024-01-12 14:00 Myriad: filesystem access restored, jobs tentatively expected for Monday

    We've restored read-write access to Myriad's filesystem, and you will be able to log in and 
    see all your directories again. Here's some detail about how you can identify which of your 
    files were damaged.

    **Your data on Myriad**

    During the incident all files that resided at least partially on OST00 have been lost. 
    In total these are 70M files (out of a filesystem total of 991M).

    We restored files in `/lustre/home` from the latest backup where available. Data that was 
    created while the backup was running or afterwards had no backup and could not be restored. 
    For the time being, we will keep the latest backup available read-only in the directory 
    `/lustre-backup`.

    Files in `/lustre/scratch` and `/lustre/projects` were not backed up, as a matter of policy. 
    All OST00 files from these directories have been lost.

    Where files were lost but still showing up in directory listings, we have removed ("unlinked") 
    them so it doesn't appear that they are still there when they are not.

    For a tiny fraction of the lost files (0.03%), there is still some file data accessible, but 
    most of these files are damaged (e.g. truncated or partially filled with zeroes). For some 
    users these damaged files might still contain useful information, so we have left these files 
    untouched.

    The following files have been placed into your home directory:

     - OST00-FILES-HOME-restored.txt
         - A list of your home directory files that resided on OST00 and that were successfully 
         restored from backup.

     - OST00-FILES-HOME-failed.txt
         - A list of your home directory files that resided on OST00 and that could not be restored 
           from backup, including one of the following messages:
         - "no backup, stale directory entry, unlinked" - There was no backup for this file, and we 
           removed the stale directory entry.
         - "target file exists, potentially corrupt, leaving untouched" - Original file data is still 
           accessible, but likely damaged or corrupt. Feel free to delete these files if there's no 
           useful data in there.

     - OST00-FILES-SCRATCH.txt
         - A list of your Scratch directory files that resided on OST00, including one of the 
           following messages (similar to the above):
         - "stale directory entry, unlinked"
         - "file exists, potentially corrupt, leaving untouched"

    For projects, the following file has been placed in the project root directory:

     - OST00-FILES-PROJECTS.txt
         - A list of project files that resided on OST00, including one of the following messages:
         - "stale directory entry, unlinked"
         - "file exists, potentially corrupt, leaving untouched"

    A very few users had newline characters (`\n`) in their filenames: in this case in the above 
    .txt files the `\n` has been replaced by the string `__NEWLINE__`, and an additional .bin file 
    has been placed alongside the .txt file, containing the list of original filenames terminated 
    by null bytes (and not including the messages).

    These OST00-FILES-* files are owned by root, so that they don't use up any of your quota. 
    You can still rename, move, or delete these files.

    **Jobs**

    We're currently running a Lustre filesystem check in the background. Provided it does not throw 
    up any serious problems, we expect to be able to re-enable jobs during Monday. We'll be putting
    user holds on all the jobs so you can check that the files and applications they are trying to 
    use exist before allowing them to be scheduled. They will show in status `hqw` in qstat.

    Once you have made sure they are ok, you will be able to use `qrls` followed by a job ID to 
    release that job, or `qrls all` to release all your jobs. They will then be in status `qw` and 
    queue as normal. (Array jobs will have the first task in status `qw` and the rest in `hqw` - 
    this is normal). If you want to delete the jobs instead, use `qdel` followed by the job ID.

    **Software**

    We have successfully restored the vast majority of Myriad's software stack. I'll send a final 
    update when we re-enable jobs, but at present I expect the missing applications to be:
 
     - ABAQUS 2017
     - ANSYS (all versions)
     - STAR-CCM+ (all versions)
     - STAR-CD (all versions)

    These are all licensed applications that are best reinstalled from their original media, so 
    we'll be working through those, starting with the most recent version we had.

    Please send any queries to rc-support@ucl.ac.uk. If you've asked us for account deletions, we 
    will be starting those next week, along with new user account creations.

  - 2024-01-15 16:00 - Myriad jobs enabled

    We have now allowed jobs to start on Myriad. 

    We have reinstalled ABAQUS 2017, ANSYS 2023.R1, STAR-CCM+ 14.06.013 and STAR-CD 4.28.050. The 
    older versions of these applications are still missing at the moment.

    Your jobs that were still in the queue from before have user holds on them and show in status 
    `hqw` in qstat.

    Once you have made sure the files and applications the jobs are using are present and correct, 
    you will be able to use `qrls` followed by a job ID to release that job, or `qrls all` to 
    release all your jobs. They will then be in status `qw` and queue as normal. (Array jobs will 
    have the first task in status `qw` and the rest in `hqw` - this is normal). If you want to 
    delete the jobs instead, use `qdel` followed by the job ID.

    User deletions and new user creations are underway. We'll need to check that the 
    synchronisation to the mailing list is working correctly and people are being added and removed
    as appropriate.

#### Action required - compression or removal of data

  - 2024-10-18 13:55 - **Action required: compression or removal of data on Myriad**

    Myriad's filesystem is too full, so we need everyone to check what data they are keeping on the 
    cluster and to remove data they are not currently using. To perform effectively, the filesystem 
    needs a significant portion of empty space. As it gets fuller, performance begins to get worse 
    and then stability also decreases. 

    The filesystem is at 70% full, and we will need to stop jobs running when it reaches 75%. (1% of 
    the filesystem is 19.4 TiB).

    Keeping data unnecessarily on the system affects everyone's ability to run jobs.

    We will also be contacting those of you with large amounts of data separately.

    **How to check usage and compress data**

    You can check your quota and see how much space you are using on Myriad with the lquota command, 
    and you can see which directories are taking the most space using `du -h` which can also be run 
    in specific directories. You can tell du to only output details of the first level of directory 
    sizes with the `--max-depth=1` option.

    If you cannot remove your data from the cluster, please consider whether you can archive and 
    compress any of it for the time being.

    Example to tar up and gzip compress a directory:

    - `tar -czvf /home/username/Scratch/myarchive.tar.gz /home/username/Scratch/work_data` 
      will (c)reate a g(z)ipped archive (v)erbosely in the specified (f)ilename location. The 
      contents will be everything in this user's `work_data` directory.

    You can request bzip2 compression instead with `-j` or `--bzip2` and `xz` compression with `-J` 
    or `--xz`. These will give you better compression than gzip, with xz generally being the most 
    compressed.

    If you are compressing an individual file rather than a directory, you can use the `gzip`, 
    `bzip2` and `xz` commands on their own without tar.

    (Have a look at `man tar` or gzip, bzip2 and xz for the manual page details - they also contain 
    the names of the uncompress commands).

    If you are transferring data to Windows and want to uncompress the data there, 7-zip can open 
    all of these formats. Or you can create your archives using the `zip` command.

    **Quota policies**

    We are going to be adjusting the policies for granting Scratch quota increases - the CRAG will 
    be granting them for shorter periods of time and will not be as easily re-granting increases at 
    the end of that time period. We will have a process for what happens when a quota increase 
    expires, including notifications to you. You will be asked to consider your plans for what to 
    do with the data when the quota increase expires at the time of requesting one.

    We are also going to be setting up annual account reapplications again, so we can identify 
    users who are no longer using the system and activate our account deletion policies.

    This is to prevent the current situation where new users to the system cannot get relatively 
    small and short term quota increases necessary for their work because increases granted in the 
    past are still using the space.

    **ARC Cluster File System (ACFS)**

    Timelines for access to the ARC Cluster Filesystem (ACFS) from Myriad are currently being 
    considered. There is some info about the ACFS at 
    [ARC Cluster File System](https://www.rc.ucl.ac.uk/docs/Background/Data_Storage/#arc-cluster-file-system-acfs) 

    The ACFS once available from Myriad will give you 1T in total of backed-up data (and your home 
    directories will no longer be backed up). For those of you with larger Scratch quota increases, 
    you will still need to consider what you will do with the bulk of your data. 

    **Info**

    We recently added some pages to our documentation which may be relevant:

    - [Parallel Filesystems](https://www.rc.ucl.ac.uk/docs/Background/Parallel_Filesystems/) includes 
      sections on working effectively with parallel filesystems and tips for use.
    - [Data Storage](https://www.rc.ucl.ac.uk/docs/Background/Data_Storage/) - what storage locations exist.
    - [Data Management](https://www.rc.ucl.ac.uk/docs/Data_Management/) - checking quotas, transferring 
      data to other users, requesting data from those who have left.

    Please email rc-support@ucl.ac.uk with any queries or raise a request about Myriad via 
    [UCL MyServices](https://myservices.ucl.ac.uk/). If you are no longer using the cluster and 
    wish to be removed from this mailing list, please also contact us and say we can delete your account.

  - 2024-10-25 18:20 - Filesystem issues on Myriad

    We have been having some filesystem issues since around 16:50.

    One of the servers that is part of the filesystem kept crashing, we had a failover and then the new 
    active one crashed as well. This will be preventing logins and jobs will have failed.

    Depending on what is happening, we may not be able to sort this out until Monday and if access is 
    restored it may be unstable throughout the weekend.

  - 2024-10-28 10:20 - Filesystem recovered

    The filesystem recovered on Friday evening and was running ok over the weekend. 

    While it was down, it is likely that many running jobs failed with I/O errors. There will also be 
    numbers of them in state `dr` which we need to reboot the nodes to clear.

    At the moment we're still seeing some leftover activity in the logs but there don't appear to be any 
    active problems.

  - 2024-11-12 17:20 - **Myriad filesystem and refresh news; ACFS available now**

    **The ARC Cluster File System (ACFS) is now available on Myriad.**

    The ARC Cluster File System (ACFS) is ARC's centralised storage system that will be available from 
    multiple ARC systems. If you also have a Kathleen account and already put some data in the ACFS, you 
    will now be able to see that data on Myriad too.

    It is the backed-up location for data which you wish to keep.

    The ACFS is available read-write on the login nodes but read-only on the compute nodes. This means 
    that your jobs can read from it, but not write to it, and it is intended that you copy data onto it 
    after deciding which outputs from your jobs are important to keep.

    * Location: `/acfs/users/<username>`
    * Also at: `/home/<username>/ACFS` (a shortcut or symbolic link to the first location).
    * Backed up daily.
    * 1T quota - no quota increases available on ACFS at present.
    * Check your ACFS quota with `aquota`.

    The ACFS has dual locations for resilience, and as a result commands like `du -h` or `ls -alsh` will 
    report filesizes on it as being twice what they really are. The `aquota` command will show you real 
    usage and quota.

    * [Data Storage - ACFS](Background/Data_Storage.md#arc-cluster-file-system-acfs)
    * [Data Management - ACFS quotas](Data_Management.md#acfs-quotas)

    **Until Myriad's filesystem is replaced, your home will continue to be backed up.** After the new 
    filesystem is available, only the ACFS will be backed up.

    Please move some of your current data to the ACFS to reduce the usage on Myriad's Scratch.

    **Myriad refresh and new filesystem**

    The Myriad refresh is going ahead, and we will be adding some new compute nodes to Myriad as well 
    as replacing the filesystem, networking and admin nodes.

    We expect the new filesystem to go live during March 2025. Prior to that, we will be conducting 
    setup, tuning and testing behind the scenes. (We expect the hardware to arrive in December or first 
    week of January depending on delivery windows).

    We expect the new compute nodes to also go live during March 2025 - they will be purchased and 
    arrive later than the filesystem, but their testing period should coincide.

    The new filesystem will be GPFS. The new compute nodes will be AMD EPYC 64 core, ~750G RAM, with 
    ~950G local disk for tmpfs. Some of the oldest nodes will be removed (eg types H, I, J which are 
    nodes named node-h, node-i, node-j).

    Timescales are based on our current knowledge and best estimates!

    There will be many software changes as part of this refresh as well - we will be updating the 
    operating system, changing the scheduler to Slurm and refreshing the software stack. More details 
    nearer the time.

    **Action required - compression or removal of data**

    Myriad's filesystem is still at 70% full and we need to stop jobs at 75% full. Please continue to 
    look at [our previous message with full info](#action-required-compression-or-removal-of-data) and 
    move data to the ACFS, off-cluster, or compress it if possible.

  - 2025-02-10 - **OS, software stack, scheduler and hardware update news**

    This is to keep you informed about upcoming changes to Myriad.

    **Live now:**

    * Research Data Storage Service mount point on login nodes
    * Open OnDemand pilot access available on request until OS upgrade

    If you have an RDSS project, you can now access that read-write on the Myriad login nodes at `/rdss`. 
    There are subdirectories `rd00`, `rd01` that contain projects with names beginning 
    `ritd-ag-project-rd00` or `ritd-ag-project-rd01` and so on. You cannot access it from the compute 
    nodes. You should `cp` or `rsync` data to your home, Scratch or the ACFS via the login nodes before 
    doing any computation using the data.

    If you want to access our [Open OnDemand pilot](https://www.rc.ucl.ac.uk/docs/Supplementary/OnDemand/) 
    for a remote desktop, Jupyter Notebooks or RStudio, contact rc-support@ucl.ac.uk and we will give you 
    access. Please fill in the feedback survey if you use it. This will be used to prioritise whether we 
    redo the work to set it up after the OS upgrade or concentrate on other improvements.

    **Incoming:**

    * OS upgrade to RHEL 9.5
    * Scheduler move to Slurm
    * New software via Spack
    * Alteration to hardware refresh schedule

    We will be moving to RHEL 9.5 as our operating system and Slurm as our new scheduler. The new 
    filesystem below will be available before this is rolled out on Myriad.

    If you have access to paid priority time on Myriad, we will stop using Gold for this and use Slurm 
    mechanisms for priority allocations instead. You will receive the equivalent resource.

    Our user documentation will be updated with examples of Slurm jobscripts instead of the SGE ones we 
    currently have. Slurm uses `srun` and `sbatch` commands instead of `qsub`, if you have come across 
    those in other software documentation.

    The OS upgrade should allow you to run more recent binaries that currently give errors about GLIBC 
    being too old. System tools will be newer and may look a little different.

    We are also creating a new small software stack built with Spack. This will available to you to test 
    before the OS upgrade, then rebuilt again after it, so it will change slightly in the meantime. Do 
    let us know if applications you use in it are missing options we currently have or not working as 
    you expect. 

    Most of the existing software stack will be reinstalled for the new OS. We are looking to remove 
    some of the oldest installs and modules that are not being used in jobs. We then intend to prune 
    this further over time and add newer versions into the Spack stack.

    **Alteration to hardware refresh schedule**

    The oldest nodetypes (H,I,J) in Myriad have been drained of jobs as planned and are being removed 
    to make space in the racks.

    The new Myriad filesystem is in place, undergoing its final testing period and we should have 
    information on 24 Feb for timescales when you can expect to begin using it.

    We are still adding new hardware to Myriad for general use, but it will not be added in March/April 
    as previously stated. Instead, we will be replacing all Myriad's network hardware first, and then 
    getting the new standard compute nodes in the next UCL financial year after August 2025. The specs 
    for the new compute nodes are:

    * AMD EPYC 9554P 64C 360W 3.1GHz (64 cores)
    * 768G RAM
    * 2x 480GB M.2 7450 PRO NVMe SSD (960G local disk)

    Note, this does not affect timelines for any paid nodes and those can still go in before this. It 
    is to split the purchase over financial years rather than any issue with hardware supply times.

    There is some additional hardware that we may be able to make available to increase general use 
    Myriad capacity in the intervening period. The details need working out and that will take a few 
    months. The OS and scheduler upgrade will need to happen first.

    **Maintenance day Tues 11 Feb**

    The nodes are being drained for a system config change this maintenance day - they'll be rebooted 
    and jobs restarted after each is updated. There will also be a reboot of switches in the ACFS that 
    will cause the ACFS mount on Myriad to hang for a period during the day. This is listed at 
    [Planned Outages](https://www.rc.ucl.ac.uk/docs/Planned_Outages/ )

    **Documentation links**

    The SSL certificate for www.rc.ucl.ac.uk is due to expire at midnight on 12 Feb. We're getting a 
    new one but there might be a gap if it can't be renewed in time. If that happens your browser may 
    prevent you accessing that link because the certificate is expired.

    * [mkdocs-rc-docs on Github](https://github-pages.arc.ucl.ac.uk/mkdocs-rc-docs/) will work

    If there is a gap when the certificate expires we'll update the links in the message you see when 
    you log in to the cluster, but if you are using an existing link or bookmark for www.rc.ucl.ac.uk 
    at that point you will get an error or warning about the expired certificate.

    (This did not happen, the certificate was renewed in time).


    2025-03-05 - **Myriad new filesystem update**

    The Myriad new filesystem is going to have a system update before we put it into production - the 
    new version fixes a number of security vulnerabilities, and it will be less disruptive to your 
    jobs if we do this before you have access to it. We also have some minor hardware issues that have 
    failed deployment checks that we are getting resolved with our vendors.

    We're currently expecting to be able to give you access to the new filesystem around **31 March**, 
    assuming all goes well with the above updates and the remaining snagging issues.

    **What will happen?**

    When the new filesystem goes live, you'll log in and will see a new empty home and Scratch. Your 
    old home and Scratch will be available read-only at `/old_lustre/home/username` and 
    `/old_lustre/scratch/username`. You'll be able to copy or rsync data out of it to the new 
    filesystem, to the ACFS or archive it elsewhere. You will not be able to modify or delete the data 
    in `/old_lustre`.

    Your new home directory will not be backed up. The ACFS will remain as your backed-up location.

    `/old_lustre` will remain available for three months after the new filesystem go-live date and will 
    then be removed. 

    All jobs will be held in the queue and you'll be able to remove the holds yourself when the data 
    they need is in the right place on the new filesystem.

    Information on how best to do the data moving will be sent nearer the time and added to the documentation.

    **Quota expiry on the new filesystem**

    For the new filesystem we will be updating our policy on what happens when increased quotas expire 
    and are not renewed. This will involve moving your user data off Myriad's filesystem to another 
    location temporarily, and then deletion of it on specified timescales. This is to avoid the new 
    filesystem filling up with data which is no longer being worked on and to allow those of you who are 
    actively using an increased quota to be able to have the space you need.

    Myriad's filesystem is not a long-term data store - if you are using the data in your jobs, that is 
    fine. If you are no longer using Myriad to do computations on the data, it shouldn't be left on 
    Myriad's filesystem.

    You will receive multiple notifications before and after your quota expires if this is happening.

    Further details on this to come. A similar process will take place when your Myriad user account 
    expires.

#### Myriad new filesystem

  - 2025-03-31 - **Myriad new filesystem on Mon 7 April**

    We are replacing Myriad's filesystem with the new one on Monday 7 April.

    From 9am there will be a maintenance period when you will not be able to log in while we switch over to the new filesystem and do final checks. We expect to give you access again later during the 7 April, but if it takes longer you may not have access until Tues 8 April.

    Only the data on ACFS will be backed up. Please note that the data on the new filesystem will not be backed up, 
    not even data under `/home`.

    **After the maintenance, you will have the following storage locations:**
 
    - `/home/username`: your new home directory on the new filesystem; not backed up
    - `/home/username/Scratch`: we are keeping this as a directory in your home for convenience. It is now part of the same space with the same rules and quota as the rest of your home directory, still not backed up.
    - `/home/username/oldhome`: a symbolic link to `/old_lustre/home/username`, your old home directory on the old filesystem; read-only (no changes to data possible)
    - `/home/username/oldscratch`: a symbolic link to `/old_lustre/scratch/username`, your old scratch directory on the old filesystem; read-only (no changes to data possible)
    - `/home/username/ACFS`: a symbolic link to your ACFS space where you can put data you want to be backed up; unaffected by this change

    **Quotas**

    You will have one quota of 1T on Myriad for your home by default. If you applied for a quota increase or renewal after 15 Feb 2025 we will set this quota on the new filesystem for you (also if you already applied for the increase specifically for the new filesystem). If you had a quota increase from before 15 Feb, it will not be recreated on the new filesystem straight away and you will need to apply for it again.

    **New terms for quota increases and when data is deleted**

    If you have a Myriad quota increase, it will have an expiry date, maximum of one year after application. You will be sent a reminder one month before your quota expires, and then reminders at two weeks, one week, expiry day, two weeks after, one month after. One month after a quota has expired, if you have not contacted us, we will move all the contents of your Myriad home and Scratch into another location. We will keep your data there for two months further to give you longer to contact us to reapply for quota or retrieve the data and will then delete all of it. Please consider this at the time of applying for your quota increase, especially if you expect to be away from UCL for a period of time.

    This is to prevent Myriad's filesystem from filling up from large quota increases that expire and are not removed. 
    If you are in contact with us and reapplying for your quota increases, this should allow us to keep granting them 
    to you. 

    **What you need to do**

    **Step 1: Move your data**

    After we tell you the new filesystem is available and you can log in, you will need to log in using your UCL 
    password, as any ssh keys you might have set up will not be there on the new filesystem. You can then copy your 
    `.ssh` directory from `/old_lustre/home/username` to your new home.

    Commands you may wish to use for copying:

    - Copy your old .ssh directory into your new home (~) recursively and preserve permissions:
        - `cp -rp ~/oldhome/.ssh ~`  
    - Use rsync archive mode (recursively copy directories, copy symlinks without resolving, and preserve permissions, ownership and modification times) to copy your old .ssh directory into your new home. The `--safe-links` option tells it to ignore any symbolic links that point outside the copied tree and any symlinks that are absolute paths:
        - `rsync --safelinks -r -a ~/oldhome/.ssh ~` 

    Copy only works locally, so you can use it for any filesystems that are directly mounted on Myriad (old_lustre, ACFS, RDSS, new filesystem). Rsync can be used locally or between remote systems as well. It can also resume incomplete copies by running again and doesn't re-copy data that you have already copied if your transfer gets interrupted for any reason.

    You will need to copy your data off old_lustre onto the new filesystem, or the ACFS, or the RDSS if you have a project, or onto other external systems if you want to keep it for future reference.

    If you have large amounts of data (particularly many small files) that you are intending to archive elsewhere, consider creating tar archives straight away instead of copying data recursively first.

    - `tar -czvf /home/username/Scratch/myarchive.tar.gz /old_lustre/home/username/data` will (c)reate a g(z)ipped archive (v)erbosely in the specified (f)ilename location on the new filesystem. The contents will be everything in this user's old "data" directory.

    **Step 2: release your jobs**

    All your jobs will be in held status (`hqw`) so that they do not fail while your data is not there. After you have copied the data that your jobs need to the new filesystem, you can release the hold on your queued jobs.

    - `qrls $JOB_ID` will release a specific job ID, and `qrls all` will release all your jobs.

    Released array jobs will have the first task in status qw and the rest in hqw - this is normal.

    **FAQ: .bashrc and hidden files**

    _Where is my old .bashrc? Why are my jobs failing with module errors now? Why are my python packages not there?_

    Your `.bashrc` is in `/old_lustre/home/username/.bashrc`

    It begins with a dot and is a hidden file so will only show up with `ls -a` rather than `ls`. You can copy this across into your current home again. You may have put module load and unload commands in it, so are now getting module conflicts when your jobs run since otherwise the modules are still the default ones.

    This also applies to other hidden files or directories you may have, like `.condarc` and `.python3local` where you may have environments defined or packages installed.

    _After I move my files, will they still be read-only?_

    No, it is the old filesystem itself that was set to be read-only. After you copy your files to the new filesystem, 
    you will be able to edit them in the same way as before.

    **Project/shared spaces/hosted datasets**

    If you have an existing project or shared space or hosted dataset (in `/lustre/projects` aka `/shared/ucl/depts`) you will need to reapply for this space and we will need to recreate it on the new filesystem. We'll be sending another email separately to people we have listed as the owners of spaces.

    The same terms for quota increases and data deletion set out above will apply to project spaces so the data will 
    be deleted if they expire and are not renewed, after reminders. Where an access group (eg `ag-archpc-groupname`, 
    formerly `lgsh0xx`) has access to the space, we will contact all members of the group so they are still aware if 
    the original owner of the space has left.

    If you currently have an access group named like `lgsh0xx` for your space, as part of reapplying we will be 
    transferring this to a new access group named `ag-archpc-groupname`. These groups can be updated within half 
    an hour rather than only overnight.

    **Removal of old filesystem**

    `/old_lustre` will be available for three months, until 9am on Monday 7 July. It will then be
    unmounted and you will not be able to access it any longer.

    **Myriad at risk for first week**

    Myriad should be considered exposed to potential issues for the first week of running a full
    workload with the new filesystem, and so there might be interruptions to service if anything goes
    wrong or needs tuning differently.

    The new filesystem is GPFS (IBM Storage Scale) and not Lustre, for those who are interested.

    Additional FAQs will be added here based on questions we receive.

  - 2025-04-14 - **Myriad filesystem update and issues with symlinks**

    This is a quick rundown of what else happened on Myriad last week and then some tips for problems
    people have been having.

    After the new filesystem went live, we had a few issues on Wednesday and Thursday where some jobs
    were causing nodes to crash which was in turn causing the gpfs client to hang - which you will have
    seen as timeouts or very slow access on the login nodes. The hangs also meant that a few people had
    their new home directories only half-created, so didn't have a home directory that belonged to them
    when they logged in. We changed some configuration on the compute nodes to fix the issue (the jobs
    causing the problem were running out of virtual memory). People who had the home directory issue
    should have been sorted out on Thursday and Friday - let us know if anyone else still gets an error
    about their home directory not existing.

    We were running more smoothly by Friday. Issues like these are why we said the rest of that week was
    at risk, as there was likely to be something that needed adjusting once real jobs started.

    **Symbolic links and Scratch**

    You start out with an empty normal directory called Scratch in your home. What I had not considered
    is if you rsync the whole of your oldhome back in, then it will rsync the old Scratch symlink
    (shortcut) from oldhome and replace the empty Scratch directory with it. This only happens because
    that directory is empty.

    We have had tickets from some of you about finding that files are read-only that you think you have
    copied - it is because they are still really on the old filesystem.

    If you do an ls -al in your home you will be able to see if you have ended up with something similar
    to this:

    ```
    lrwxrwxrwx   1 cceahke staff          24 Sep 10  2024 Scratch -> /lustre/scratch/scratch/cceahke
    ```

    That shows you that Scratch is a symlink and is pointing to a location on the old filesystem.

    To fix, delete the symlink and recreate Scratch as a directory:
    
    ```    
    rm Scratch
    mkdir Scratch
    ```

    You can then go ahead and rsync the contents of oldscratch into Scratch so they are copied onto the 
    new filesystem correctly. You cannot accidentally delete the contents of oldscratch since it is 
    read-only.

    If you have not rsynced your home yet, you could add the `--safe-links` option to rsync, which tells 
    it to ignore any symbolic links that point outside the copied tree and any symlinks that are 
    absolute paths. So when copying home, the symlink to `/lustre/scratch/scratch` should then be 
    ignored: 

    ```
    rsync --safe-links -r -a ~/oldhome ~
    ```

    We are catching up on the quota and shared space requests we have received.

  - 2025-05-23 10:30 - Myriad filesystem issues last night and this morning

    We needed to stop jobs running last night at around 22:30 after something happened with Myriad's filesystem
    beginning around 17:00 and getting worse by 18:00 - if you've had error messages in your jobs about "stale
    file handles" or issues using the cluster interactively this is why.

    We're continuing to work out what happened and get everything running again. We will send more information
    when we have it.

  - 2025-05-23 18:20 - Re: Myriad filesystem issues last night and this morning

    We let jobs run this afternoon on part of Myriad and are continuing to allow them on the rest of the compute
    nodes in sections so that everything should be up and running over the bank holiday weekend.

  - 2025-06-25 11:40 - Myriad old filesystem removal at 9am on Mon 7 July

    This is a reminder that your `oldhome`, `oldscratch` and any shared spaces on `/lustre` are going to be
    unmounted on 9am on Monday 7 July.

    If you know that you cannot complete copying your data by then, please let us know asap. We can grant short
    term emergency quota increases on the new filesystem in advance of July review by the CRAG to allow you to
    copy your data off there before it is removed. (If you have already sent those in, we'll be looking at them
    shortly).

    Reminder: `gquota` shows your new filesystem quota. The `lquota` command will still show you the quota for
    your old space.

#### Latest on Myriad

  - 2025-07-01 16:35 - Full outage for Myriad during maintenance window on 8 July; 2 week extension for old
    filesystem

    We will be using Myriad's next maintenance day on **Tues 8 July** to update some firmware and make some
    filesystem adjustments, and there will be a full outage all day where you will not be able to log in or
    access the filesystem. We are draining jobs for 8am that day to allow those tasks to be carried out.

    A job drain means that if your job won't have time to finish before the outage, it will not start and will
    remain in the queue until afterwards, when it will be scheduled as normal. You do not need to take any action.

    **2 week extension for old filesystem**

    We have also had several requests to extend the time period for access to oldhome and oldscratch on the old
    filesystem, which was due to be removed at 9am on 7 July. We are extending this for two weeks until
    **9am on Mon 21 July**. This is to give you time to finish copying your data onto the new filesystem.
    Your access will be interrupted during the maintenance period above.

  - 2025-07-08 12:00 - Today's outage postponed

    This outage has now been postponed and jobs have been re-enabled on Myriad.

    We will likely need to reschedule it before the next maintenance period, but will let you
    know once we have had some clarification from our vendors.

    Apologies for the interruption!
    

### Kathleen

  - 2024-09-05 - Kathleen outage for new filesystems on Tues 10 September - action required 

    The previously-announced Kathleen outage for new filesystems will now take place on maintenance 
    day next week, **Tuesday 10 September**. 

    The Kathleen cluster will go into maintenance on Tuesday 10 Sept 2024 from 9:00. Logins to 
    Kathleen will not be possible until the maintenance is finished. Any jobs that won’t finish by 
    the start of the maintenance window will stay queued. We aim to finish the maintenance within 
    one day, so that you can access Kathleen again on Weds 11 Sept.
 
    **We are implementing a number of changes to how data is stored on Kathleen:**

    * The current Lustre filesystem will be replaced with a new Lustre filesystem. The old Lustre is 
      running on aging and error-prone hardware, and suffers from performance issues, especially for 
      interactive work on the login nodes. The new Lustre should provide a vastly better experience.
    * The Kathleen nodes will mount the ACFS (ARC Cluster File System) which is ARC’s new centralised 
      storage system that will (eventually) be available on other ARC systems (e.g. Myriad) too. 
      ACFS will be available read-write on the login nodes but read-only on the compute nodes.
    * Going forward, only the data on ACFS will be backed up. **Please note that the data on the new 
      Lustre will not be backed up, not even data under `/home`**.
 
    **After the maintenance, you have the following storage locations:**

    * `/home/username`: your new home directory on the new Lustre; not backed up (this is a change 
      to the current situation)
    * `/scratch/username`: your new scratch directory on the new Lustre; not backed up
    * `/acfs/users/username`: your ACFS directory; backed up daily; read-only on the compute nodes
    * `/old_lustre/home/username`: your old home directory on the old Lustre; read-only
    * `/old_lustre/scratch/username`: your old scratch directory on the old Lustre; read-only

    You will also have a `~/ACFS` shortcut/symbolic link in your home that points to `/acfs/users/username`.
 
    If you are looking in your `/old_lustre/home/username/Scratch` symbolic link, that will direct you 
    back to your **new** Scratch not your old Scratch because it uses an absolute path. Please make sure 
    to access old Scratch using `/old_lustre/scratch/username`.

    **What you will need to do (after the maintenance):**

    * After login, you will notice that your new home and scratch directories are mostly empty. 
      Please copy any data you need from your old home and scratch directories under `/old_lustre` to 
      the appropriate new locations.
        - E.g. `cp -rp /old_lustre/home/username/data /home/username` will recursively copy your old 
          `data` directory and everything in it into your new home, while preserving the permissions.
    * Any data that you consider important enough to be backed up should be copied into your ACFS 
      directory instead.
    * You have **three months** to copy your data. After this, the `/old_lustre` will become unavailable.
    * Your queued jobs will be held (showing status `hqw` in `qstat`) and won’t start running 
      automatically, as their job scripts will likely refer to locations on `/lustre` which won’t exist 
      until you have copied over the data. After you have copied the data that your jobs need to the new 
      Lustre, you can release the hold on your queued jobs.
        - E.g. `qrls $JOB_ID` will release a specific job ID, and `qrls all` will release all your jobs.
        - Released array jobs will have the first task in status `qw` and the rest in `hqw` - this is normal.
    * Depending on the amount of data, the copying may take some time, especially if you have many small 
      files. If you are copying data to ACFS and you don’t need immediate access to each file individually, 
      consider creating tar archives instead of copying data recursively.
        - E.g. `tar -czvf /acfs/users/username/myarchive.tar.gz /old_lustre/home/username/data` will 
          (c)reate a g(z)ipped archive (v)erbosely in the specified (f)ilename location. The contents will be 
          everything in this user's old `data` directory. 

    Further reminders will be sent before `/old_lustre` is removed on 11 December.

    **Kathleen quotas**

    You will continue to have one quota on Kathleen, with a default value of 250G that includes your 
    home and Scratch. If you have an active quota increase request that has not reached its requested 
    expiry date on your old space, we will be recreating these on the new space. As stated above, 
    `/home` will no longer be backed up.

    **ACFS quotas**

    You will have 1T of quota on the ACFS. You will be able to check this with the `aquota` command.

    The ACFS has dual locations for resilience, and as a result standard commands like `du` or `ls -al` 
    will report filesizes on it as being twice what they really are. The `aquota` command will show you 
    real usage and quota. One of the reasons for the previous delay is that we tried to get filesizes to 
    report correctly in all circumstances, but that was not possible so we decided it was less confusing 
    if everything other than `aquota` always showed double. 

    For those interested, the ACFS is a GPFS filesystem.

    This outage will show shortly on [Planned Outages](https://www.rc.ucl.ac.uk/docs/Planned_Outages/) 
    and the ACFS information will be added to our documentation.

    We apologise for the inconvenience, but we believe these changes will help to provide a more performant 
    and robust service in the future.
 
    Please email rc-support@ucl.ac.uk with any queries or raise a request about Kathleen via 
    [UCL MyServices](https://myservices.ucl.ac.uk/).

  - 2024-09-11 09:35 Kathleen filesystem outage complete

    The outage is complete and you can log in and access your new home, Scratch and ACFS directories 
    on Kathleen.

    In addition to the information given previously:

    - You will have a `~/ACFS` shortcut/symbolic link in your home that points to `/acfs/users/username`
    - If you want to copy data and preserve the permissions, use `cp -rp` rather than `cp -r`
    - If you are looking in your `/old_lustre/home/username/Scratch` symbolic link, that will direct you 
      back to your new Scratch not your old Scratch because it uses an absolute path. Please make sure to 
      access old Scratch using `/old_lustre/scratch/username`

    This will be added to https://www.rc.ucl.ac.uk/docs/Status_page/#kathleen and has been updated in the 
    original message there.

    The message you see when first logging in to Kathleen has been updated with the change to backed-up 
    locations and ACFS information.

    We have these additional pages in our documentation:

    - https://www.rc.ucl.ac.uk/docs/Background/Parallel_Filesystems/ (parallel filesystem concepts)
    - https://www.rc.ucl.ac.uk/docs/Background/Data_Storage/ (data storage locations we provide)
    - https://www.rc.ucl.ac.uk/docs/Data_Management/ (how to check quotas, transfer ownership of files)

    **Terms & Conditions update**

    We have updated our Terms and Conditions (https://www.rc.ucl.ac.uk/docs/Terms_and_Conditions/) - 
    please take a look. It now defines our data retention policies and when we can access your data, 
    among other things. 

  - 2024-11-28 14:00 - **Reminder: Kathleen /old_lustre removal on 11 Dec; later upcoming changes**

    This is a reminder that access to `/old_lustre` will be removed on **Monday 11 December**, so if 
    you still have files in your old home and scratch directories, they will no longer be accessible 
    after this time.

    Here's the previous information sent out on how to check that and how to copy your data: 
    https://www.rc.ucl.ac.uk/docs/Status_page/#kathleen

    Note: do check for hidden files starting with a `.` as well, such as customisations you may have 
    added to your `.bashrc`, config files for programs like `.vimrc` and other directories like `.conda`, 
    `.python3local`, `.julia`, `.cpanm` where you may have installed packages or have other 
    environments or configuration.

    These are visible to `ls -a` but not to `ls`.

    **Later upcoming changes**

    You may be aware that we are working to update the operating system on all our clusters to RHEL 9. 
    Kathleen is likely to be the first deployed. There will be more details nearer the time, but this will 
    involve an outage and after it the operating system will be updated, the software will be rebuilt and we 
    will have Slurm as our scheduler instead of SGE.

    We've had questions from some of you about VSCode, since it will stop connecting to unsupported 
    operating systems in February 2025 - once we have updated the operating system it will be able to connect 
    again. 

    Right now we don't have a timescale for this but will be letting you know when we do. Development work 
    is ongoing. Documentation will be updated for what you will need to do with jobscripts and job submission 
    commands. This is just to let you know that these changes are coming.

  - 2024-12-11 09:10 - Kathleen /old_lustre removal

    11 Dec is not a Monday, contrary to my previous email. As a result, we will leave `/old_lustre` 
    mounted on Kathleen until **Monday 16 December**. If you still have files in your old home and 
    scratch directories, they will no longer be accessible after this time. It will be unmounted at
    or shortly after 9am.

  - 2025-02-10 - **OS, software stack and scheduler update news**

    This is to keep you informed about upcoming changes to Kathleen.

    **Live now:**

    * Research Data Storage Service mount point on login nodes

    If you have an RDSS project, you can now access that read-write on the Kathleen login nodes at `/rdss`.
    There are subdirectories `rd00`, `rd01` that contain projects with names beginning
    `ritd-ag-project-rd00` or `ritd-ag-project-rd01` and so on. You cannot access it from the compute
    nodes. You should `cp` or `rsync` data to your home, Scratch or the ACFS via the login nodes before
    doing any computation using the data.

    **Incoming:**

    * OS upgrade to RHEL 9.5
    * Scheduler move to Slurm
    * New software via Spack

    We will be moving to RHEL 9.5 as our operating system and Slurm as our new scheduler.

    We will send more updates when we are ready to roll it out - more info should be coming in the next 
    couple of weeks. It will be available on Kathleen first.

    Our user documentation will be updated with examples of Slurm jobscripts instead of the SGE ones we
    currently have. Slurm uses `srun` and `sbatch` commands instead of `qsub`, if you have come across
    those in other software documentation.

    The OS upgrade should allow you to run more recent binaries that currently give errors about GLIBC
    being too old. System tools will be newer and may look a little different.

    We are also creating a new small software stack built with Spack. This will available to you to test
    before the OS upgrade, then rebuilt again after it, so it will change slightly in the meantime. Do
    let us know if applications you use in it are missing options we currently have or not working as
    you expect.

    We are aware there is a new CASTEP to add, and we can't install the recommended newer Intel OneAPI 
    compilers to build the newest VASP until after the OS upgrade.

    Most of the existing software stack will be reinstalled for the new OS. We are looking to remove
    some of the oldest installs and modules that are not being used in jobs. We then intend to prune
    this further over time and add newer versions into the Spack stack.

    **Documentation links**

    The SSL certificate for www.rc.ucl.ac.uk is due to expire at midnight on 12 Feb. We're getting a
    new one but there might be a gap if it can't be renewed in time. If that happens your browser may
    prevent you accessing that link because the certificate is expired.

    * [mkdocs-rc-docs on Github](https://github-pages.arc.ucl.ac.uk/mkdocs-rc-docs/) will work

    If there is a gap when the certificate expires we'll update the links in the message you see when
    you log in to the cluster, but if you are using an existing link or bookmark for www.rc.ucl.ac.uk
    at that point you will get an error or warning about the expired certificate.

    (This did not happen, the certificate was renewed in time).

  - 2025-02-17 - **New test software stack available**

    There is a test version of our next software stack available now on Kathleen. This has a small 
    number of packages at present. What is in it and the names of modules are liable to change over 
    time, so please do not rely on it for production work. Instead, please test whether the 
    applications you intend to use work the way you would expect.

    This stack is built using [Spack](https://spack.readthedocs.io/en/latest/).

    To use:

    ```
    module load beta-modules
    module load test-stack/2025-02
    ```

    After that, when you type `module avail` there will be several sections of additional modules at 
    the top of the output.

    Not everything contained in the stack is visible by default - we have made the applications that 
    we expect people to use directly visible and lots of their dependencies are hidden. These will 
    show up if you search for that package specifically, for example:

    ```
    module avail libpng
    -------------------------- /shared/ucl/apps/spack/0.23/deploy/2025-02/modules/applications/linux-rhel7-cascadelake --------------------------
    libpng/1.6.39/gcc-12.3.0-iopfrab
    ```

    This module does not show up in the full list but is still installed. It has a hash at the end 
    of its name `-iopfrab` and this will change over time with different builds.

    If you find you are needing one of these modules often, let us know and we'll make it one that 
    is not hidden in the next release of this stack.

    The stack will be rebuilt for the operating system upgrade and moved out from beta-modules.

    This information is available at 
    [Kathleen test software stack](https://www.rc.ucl.ac.uk/docs/Clusters/Kathleen/#test-software-stack)

  - 2025-06-06 12:10 - Kathleen operating system and scheduler upgrade

    We are intending to upgrade half of Kathleen to the new operating system (RHEL9) and scheduler
    (Slurm) and give you access on **Weds 18 June** if all goes well.

    We are draining half of the compute nodes in preparation for this.

    We intend to run old Kathleen and new Kathleen in parallel for a while to allow any problems to
    be found, more software to be installed, and for you to gradually move over to the new environment
    while still being able to use the old one so your work is not interrupted.

    Maximum jobsize on old Kathleen will be 70 nodes during this time.

    We will give you instructions on how to access new Kathleen login nodes and what the Slurm
    jobscripts should look like nearer the time.

    We will not have Apptainer available straight away - we intend to add it later.

#### Latest on Kathleen

  - 2025-06-18 17:05 - Kathleen operating system and scheduler upgrade

    The upgraded half of Kathleen is now available.

    To access, log in to `kathleen-ng.rc.ucl.ac.uk` instead of `kathleen.rc.ucl.ac.uk`. This is a
    round-robin address for two login nodes. If you are outside UCL, use the ssh-gateway or VPN as
    usual.

    If you need to access the new login nodes individually, for now you can use these addresses:
    ```
    login21.kathleen.rc.ucl.ac.uk
    login22.kathleen.rc.ucl.ac.uk
    ```

    We have updated documentation at:

    - https://www.rc.ucl.ac.uk/docs/Clusters/Kathleen/ - contains info on the current split state and links to the following.
    - https://www.rc.ucl.ac.uk/docs/Supplementary/Slurm/ - has the Slurm commands
    - https://www.rc.ucl.ac.uk/docs/Supplementary/Slurm_Example_Jobscripts/ - has a couple of example jobscripts

    The new software stack is provided by the `ucl-stack/2025-05` module, and we have a new
    `default-modules/2025-05` that should also be loaded by default. We have made a change to the
    top of everyone's `.bashrc` to make sure that the different default-modules are set
    appropriately on both old and new Kathleen.

    We do not currently have screen installed - we do have tmux.

    Over the next few weeks we expect to be installing more software, fixing any issues and updating the
    documentation further.

    We expect to be running both halves of Kathleen for a month. If more work is required we may run old
    Kathleen for a bit longer.

    For any of you using VSCode, it should be able to connect to new Kathleen. The operating system on old
    Kathleen (and Myriad) is old enough that current versions will no longer connect there.


### Young

  - 2023-10-26 14:50 - We seem to have a dying OmniPath switch in Young. The 32 nodes with names 
    beginning `node-c12b` lost their connection to the filesystem earlier. Powercycling the switch 
    only helped temporarily before it went down again. Those nodes are all currently out of service 
    so new jobs will not start on them, but if your job was running on one of those when the two 
    failures happened those jobs will have failed.

    (You can see in `jobhist` what the head node of a job was, and the .po file will show all the 
    nodes that an MPI job ran on).

  - 2024-01 Parallel filesystem soon to be replaced.

  - 2024-01-03 12:30 Filesystem issues on Young: temporary hangs, running but degraded 

    We've just had two periods today where Young's filesystem would have hung - hopefully briefly 
    enough that operations in progress will have continued after it recovered.

    We failed over from one server to the other and back again. 

    Young's filesystem is more at risk than usual right now since we have some failed disks and 
    one area (one Object Store Target) is degraded. We have stopped new data from being written 
    there and are migrating the existing data to the rest of the filesystem. 

    The filesystem is still working and Young is still running jobs. We do not need you to take 
    any action at present, but things may be running more slowly while this completes.

  - 2024-09 Young's new filesystem is being readied for service.

#### Young new filesystem

  - 2024-09-27 - Young and Michael outage for new filesystem on Mon 7 Oct - action required

    We will be replacing the two filesystems on Young and Michael with one new filesystem on 
    **Monday 7 October 2024**. 
 
    Both clusters will go into maintenance on Monday 7 Oct 2024 from 09:00am. Logins will not be 
    possible until the maintenance is finished. Any jobs that won’t finish by the start of the 
    maintenance window will stay queued. We aim to finish the maintenance within one day, so that 
    you can access the clusters again on Tues 8 Oct.

    **Single login node outages between 2-4 Oct**

    From 2-4 October, Young `login02` and Michael `login11` will each be out of service for a day 
    during this period for testing updates before the filesystem migration. There will be no 
    interruption to jobs or logins to the general addresses `young.rc.ucl.ac.uk` and 
    `michael.rc.ucl.ac.uk`. If you are on `login02` or `login11` at the time, you may see a message 
    that it is about to go down for reboot, and if you have a tmux or screen session on that login 
    node then it will be terminated. You will be able to log back in and be assigned to the other 
    login node, Young `login01` or Michael `login10`.
 
    **Why the change:**
 
    * Young's filesystem is running on aging and error-prone hardware, and suffers from performance 
      issues, especially for interactive work on the login nodes. The new Lustre should provide a 
      vastly better experience.
    * Michael's filesystem is old and replacement parts are no longer available.
    * The new filesystem is a HPE ClusterStor Lustre filesystem and will enable both machines to keep 
      running in a supported and maintainable manner.
 
    **After the maintenance, you have the following storage locations:**
 
    * `/home/username`: your new home directory on the new Lustre; backed up
    * `/scratch/username`: your new scratch directory on the new Lustre; not backed up
    * `/old_lustre/home/username`: your old home directory on the old Lustre; read-only
    * `/old_lustre/scratch/username`: your old scratch directory on the old Lustre; read-only

    If you currently have accounts on both Young and Michael, you will need to log into Young to 
    see Young's `old_lustre` and into Michael to see Michael's `old_lustre`, but your home and 
    Scratch will be the same on both, and the data you copy into it will be visible on both.

    **Quotas**

    On the new filesystem we are able to set separate home and Scratch quotas. 

     * Home: 100G, backed up
     * Scratch: 250G by default

    Previously the default quota was 250G total.

    If you have an existing non-expired quota increase, we will increase your Scratch quota on 
    the new filesystem to this amount. If you find you need an increased Scratch quota, you can 
    run the `request_quota` command on either cluster and it will ask you for some information and 
    send us a support ticket.
 
    **What you will need to do (after the maintenance):**
 
    * After login, you will notice that your new home and scratch directories are mostly empty. 
      Please copy any data you need from your old home and scratch directories under `/old_lustre` to 
      the appropriate new locations. Your existing SSH keys will all have been copied in so that you 
      can log in. You can do this copy on the login nodes.
        - E.g. `cp -rp /old_lustre/home/username/data /home/username` will recursively copy with 
          preserved permissions your old `data` directory and everything in it into your new home.
    * You have **three months and one week** to copy your data. After this, the `/old_lustre` will 
      become unavailable.
    * Your queued jobs will be held (showing status `hqw` in qstat) and won’t start running 
      automatically, as their job scripts will likely refer to locations on `/lustre` which won’t 
      exist until you have copied over the data. After you have copied the data that your jobs 
      need to the new Lustre, you can release the hold on your queued jobs.
        - E.g. `qrls $JOB_ID` will release a specific job ID, and `qrls all` will release all your jobs.
        - Released array jobs will have the first task in status `qw` and the rest in `hqw` - this is normal.
    * Depending on the amount of data, the copying may take some time, especially if you have many 
      small files. If you wish to archive some of your data, consider creating tar archives straight 
      away instead of copying data recursively.
        - E.g. `tar -czvf /home/username/Scratch/myarchive.tar.gz /old_lustre/home/username/data` will 
          (c)reate a g(z)ipped archive (v)erbosely in the specified (f)ilename location. The contents 
          will be everything in this user's old `data` directory. 
 
    Further reminders will be sent before the `/old_lustre` locations are removed on **14 January 2025**.

    **Terms & Conditions update**

    We have updated our [Terms and Conditions for all services](https://www.rc.ucl.ac.uk/docs/Terms_and_Conditions/) 
    - please take a look. It now defines our data retention policies and when we can access your data, 
    among other things.
  
 
    These outages are listed on [Planned Outages](https://www.rc.ucl.ac.uk/docs/Planned_Outages/). 
    The information above will also be copied into the https://www.rc.ucl.ac.uk/docs/Status_page/ 
    sections for Young and Michael.
 
    Please email rc-support@ucl.ac.uk with any queries. 

    If you are no longer using Young or Michael and wish to be removed from these mailing lists, 
    email us confirming that we can delete your accounts and we will do so and remove you from the lists.

  - 2024-10-07 11:00 - **Delay in returning Young GPU nodes to service**

    In addition to the above, there may be a delay in allowing the GPU nodes in Young to run jobs again - 
    we need to rebuild the existing GPU software for those, to have it on an architecture-specific section 
    of our software stack. This will result in GPU jobs remaining in the queue while we complete this. I 
    would expect this to be complete by Wednesday or Thursday but will keep you updated.

  - 2024-10-08 13:30 - **Logins are enabled.**

    Logins are enabled again.

    We're continuing to rebuild the GPU applications on Young.

  - 2024-10-09 17:00 - **GPU nodes are enabled, plus FAQ**

  The GPU applications are rebuilt so we have re-enabled jobs on the GPU nodes. Please let us know if you 
  encounter any issues or if we have missed something.

  We found that we hadn't synced keys across for users created after 26 September so they could not log in 
  - this was sorted out at 10:20am today. It affected mmm1457 to mmm1463 on Young.

  **Some frequently-asked questions about the filesystem update:**

  1. Where is my old `.bashrc`? & Why are my jobs failing with module errors now?

     This is in `/old_lustre/home/username/.bashrc` 

     It begins with a dot and is a hidden file so will only show up with `ls -a` rather than `ls`. You can 
     copy this across into your current home again. You may have put module load and unload commands in it, 
     so are now getting module conflicts when your jobs run since the modules are the default ones.

     Note that this also applies to other hidden files or directories you may have, like `.condarc` and `.python3local`.

  2. Do I need to copy my data in a job?

     No, you can do the copy on the login nodes. It can cause a lot of I/O usage on the login nodes so you 
     could also do it in a job if you wanted or if you noticed that it was going slowly, but using the login 
     nodes for this is allowed.

  3. I need more than the 100G home quota and 250G Scratch quota for my data, what do I do?

     Home is your backed-up area and is limited in size. Scratch is not backed up and also not deleted and we 
     can give you more space there. Run the `request_quota` command and it will ask you a few questions and send 
     us the request. We migrated all quota increases that were made within the last 12 months to Scratch 
     increases and did not migrate older ones. This allows us to make sure that the quota increases are current 
     and still needed. We are processing the new requests.

  4. Where should I write large temporary data during my jobs?

     Into Scratch. As above, you can request a larger quota if you need one. (If you are using the GPU nodes, 
     you can use `$TMPDIR` and request it with `#$ -l tmpfs=20G` for example, but the GPU nodes are the only 
     ones with local disks so everything else needs to be in Scratch).

  - 2024-09-04 15:10 - UKRI survey, new HBM nodes and GPU node driver updates

    **EPSRC Large Scale Facilities - User survey**

    [EPSRC Large Scale Facilities survey](https://engagementhub.ukri.org/epsrc-researchinfrastructure/lsfusersurvey/consultation/intro/)

    Please can you fill in this survey that EPSRC are running on all their Large Scale Facilities - it 
    includes events run by facilities as well as accessibility of the remote service itself, so if you 
    attended the MMM Hub Conference last week and didn't fill it in at the end, please consider it now.

    **New High Bandwidth Memory nodes**

    We have 32 new high bandwidth memory nodes in Young, each with 64 cores, 503G usable memory and 3.5T 
    available to be requested as tmpfs in your job.

    They are currently configured in cache mode, where the HBM functions as a memory-side cache for the 
    contents of the DDR memory. This should allow all applications to take advantage of it without needing 
    to be specifically configured to use it. 

    Please try this out with your applications and let us know how it is working and whether you see an 
    improvement. We have one VASP test case at present and would like to build up suitable test cases 
    with other applications, including more specific ones with VASP. Especially if you already know you 
    have been running up against memory bandwidth limitations, please contact us at rc-support@ucl.ac.uk 
    - if you have test cases and tell us where on the filesystem they are, we can get them from you. 
    After we upgrade the operating system on the cluster we will be testing other HBM modes as well, which 
    can have a greater impact if the application is aware of it.

    We will also be using this information to inform future hardware purchases for the MMM community so any 
    feedback, positive or negative, you have on these nodes would be gratefully received.

    [Information about the nodes and how to use](Clusters/Young.md#high-bandwidth-memory-nodes)

    You will need to request these nodes explicitly in your jobscripts with `#$ -ac allow=W`

    The [Maximum job resources](Clusters/Young.md#maximum-job-resources) and 
    [Node types](Clusters/Young.md#node-types) tables are also updated.

#### Latest on Young

  - 2025-01-14 11:10 Extension to availability of `/old_lustre` on Young until **9am on Tues 28 Jan**

    We've had multiple requests from people who have been unable to retrieve their data from Young's 
    old_lustre in time for the removal of the filesystem today, so we are extending the deadline for 
    two weeks.

    The old filesystem on Young will be unmounted shortly after **9am on Tuesday 28 January**.

    There have been some questions about ways to transfer files in chunks if your transfers are being 
    interrupted, without repeating copying the same files - you can use `rsync` for this as it can 
    resume incomplete copies by running again, and if you use the `-a` option ("archive") it retains 
    many of the file properties, including e.g. timestamps.

    > Use archive mode (recursively copy directories, copy symlinks without resolving, and preserve 
    > permissions, ownership and modification times):
    >
    >   `rsync -a|--archive path/to/source path/to/destination`

    (This shows that you can use `-a` or `--archive` as the option passed in).

    Alejandro Santana-Bonilla (KCL) also created a script for copying your first 10 (or other number) 
    directories, then second 10 and so on, if that is useful to you: 

    * [old_lustre_folders script](https://github.com/kcl-tscm/Young_instructions/tree/main/old_lustre_folders)


### Michael

  - 2024-01-24 16:40 - Problem on Michael's admin nodes causing DNS failures - now solved

    We have been having DNS problems on Michael today since around 14:40, meaning that scheduler 
    commands were not working and running jobs may have errors, including failed username lookups. 
    New jobs trying to start during this time are likely to have failed on start up. Running jobs 
    may have been affected, so please check the outputs for any jobs that were running during 14:40 
    to 16:30 today.

    This was caused by a problem on the admin nodes that has now been sorted out.

  - 2024-09 Michael's new filesystem (shared with Young) is being readied for service.

#### Michael new filesystem

  - 2024-09-27 - Young and Michael outage for new filesystem on Mon 7 Oct - action required

    We will be replacing the two filesystems on Young and Michael with one new filesystem on
    **Monday 7 October 2024**.

    Both clusters will go into maintenance on Monday 7 Oct 2024 from 09:00am. Logins will not be
    possible until the maintenance is finished. Any jobs that won’t finish by the start of the
    maintenance window will stay queued. We aim to finish the maintenance within one day, so that
    you can access the clusters again on Tues 8 Oct.

    **Single login node outages between 2-4 Oct**

    From 2-4 October, Young `login02` and Michael `login11` will each be out of service for a day
    during this period for testing updates before the filesystem migration. There will be no
    interruption to jobs or logins to the general addresses `young.rc.ucl.ac.uk` and
    `michael.rc.ucl.ac.uk`. If you are on `login02` or `login11` at the time, you may see a message
    that it is about to go down for reboot, and if you have a tmux or screen session on that login
    node then it will be terminated. You will be able to log back in and be assigned to the other
    login node, Young `login01` or Michael `login10`.

    **Why the change:**

    * Young's filesystem is running on aging and error-prone hardware, and suffers from performance
      issues, especially for interactive work on the login nodes. The new Lustre should provide a
      vastly better experience.
    * Michael's filesystem is old and replacement parts are no longer available.
    * The new filesystem is a HPE ClusterStor Lustre filesystem and will enable both machines to keep
      running in a supported and maintainable manner.

    **After the maintenance, you have the following storage locations:**

    * `/home/username`: your new home directory on the new Lustre; backed up
    * `/scratch/username`: your new scratch directory on the new Lustre; not backed up
    * `/old_lustre/home/username`: your old home directory on the old Lustre; read-only
    * `/old_lustre/scratch/username`: your old scratch directory on the old Lustre; read-only

    If you currently have accounts on both Young and Michael, you will need to log into Young to
    see Young's `old_lustre` and into Michael to see Michael's `old_lustre`, but your home and
    Scratch will be the same on both, and the data you copy into it will be visible on both.

    **Quotas**

    On the new filesystem we are able to set separate home and Scratch quotas.

     * Home: 100G, backed up
     * Scratch: 250G by default

    Previously the default quota was 250G total.

    If you have an existing non-expired quota increase, we will increase your Scratch quota on
    the new filesystem to this amount. If you find you need an increased Scratch quota, you can
    run the `request_quota` command on either cluster and it will ask you for some information and
    send us a support ticket.

    **What you will need to do (after the maintenance):**

    * After login, you will notice that your new home and scratch directories are mostly empty.
      Please copy any data you need from your old home and scratch directories under `/old_lustre` to
      the appropriate new locations. Your existing SSH keys will all have been copied in so that you
      can log in. You can do this copy on the login nodes.
        - E.g. `cp -rp /old_lustre/home/username/data /home/username` will recursively copy with
          preserved permissions your old `data` directory and everything in it into your new home.
    * You have **three months and one week** to copy your data. After this, the `/old_lustre` will
      become unavailable.
    * Your queued jobs will be held (showing status `hqw` in qstat) and won’t start running
      automatically, as their job scripts will likely refer to locations on `/lustre` which won’t
      exist until you have copied over the data. After you have copied the data that your jobs
      need to the new Lustre, you can release the hold on your queued jobs.
        - E.g. `qrls $JOB_ID` will release a specific job ID, and `qrls all` will release all your jobs.
        - Released array jobs will have the first task in status `qw` and the rest in `hqw` - this is normal.
    * Depending on the amount of data, the copying may take some time, especially if you have many
      small files. If you wish to archive some of your data, consider creating tar archives straight
      away instead of copying data recursively.
        - E.g. `tar -czvf /home/username/Scratch/myarchive.tar.gz /old_lustre/home/username/data` will
          (c)reate a g(z)ipped archive (v)erbosely in the specified (f)ilename location. The contents
          will be everything in this user's old `data` directory.

    Further reminders will be sent before the `/old_lustre` locations are removed on **14 January 2025**.

    **Terms & Conditions update**

    We have updated our [Terms and Conditions for all services](https://www.rc.ucl.ac.uk/docs/Terms_and_Conditions/)
    - please take a look. It now defines our data retention policies and when we can access your data,
    among other things.


    These outages are listed on [Planned Outages](https://www.rc.ucl.ac.uk/docs/Planned_Outages/). 
    The information above will also be copied into the https://www.rc.ucl.ac.uk/docs/Status_page/ 
    sections for Young and Michael.

    Please email rc-support@ucl.ac.uk with any queries.

    If you are no longer using Young or Michael and wish to be removed from these mailing lists,
    email us confirming that we can delete your accounts and we will do so and remove you from the lists.

  - 2024-10-08 13:30 Logins are enabled.

    Logins are enabled again.


### Thomas

  - Thomas is now retired.

