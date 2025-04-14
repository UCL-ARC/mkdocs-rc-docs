---
layout: docs
---

# Planned Outages

The second Tuesday of every month is a maintenance day, when the following clusters should be considered at risk from 8:00AM: Myriad, Kathleen, Young, Michael, Aristotle and the Data Science Platform. We won’t necessarily perform maintenance every month, and notice by email will not always be given about maintenance day work that only puts services at risk.

Full details of outages are emailed to the cluster-specific user lists. 

Generally speaking, an outage will last from the morning of the first date listed until mid-morning of the end date listed. The nodes may need to be emptied of jobs in advance ('drained'), so jobs may remain in the queue for longer before an outage begins.

If there is a notable delay in bringing the system back we will contact you after approximately midday - please don't email us at 9am on the listed end days!

After an outage, the first day or two back should be considered 'at risk'; that is, things are more likely to go wrong without warning and we might need to make adjustments.

Date                | Service | Status | Reason 
--------------------|---------|--------|--------
7 April 2025 | Myriad | Planned | Outage for switchover to new filesystem. No login access from 9am. Once access is restored later that day or the next day, you will have an empty new home and Scratch with symbolic links to `oldhome` and `oldscratch`. All jobs will be held. You will need to copy your data to the new filesystem or archive it. Once done, you can release the hold on your jobs using `qrls $JOB_ID` or `qrls all`.

## Previous Outages

Date                | Service | Status | Reason 
--------------------|---------|--------|--------
18 March 2025 | Myriad, Kathleen | Completed | ACFS will be unavailable and all jobs on Myriad and Kathleen will be drained for 8am. This is to do some work on the ACFS that will allow archiving in future. Had to schedule for 18th not 11th based on vendor availability. Jobs that will not be able to complete before the outage will stay in the queue and will not start until after the outage. The ACFS is at risk all day. We'll let you know if the work is completed earlier.
11 February 2025 | Myriad | Completed | Maintenance day: nodes being drained for a system config change. Also a reboot of ACFS switches will cause the ACFS mount on Myriad to hang for a period during the day.
28 January 2025 | Young, Michael | Completed | /old_lustre will be unmounted. If you still have files in your old home and scratch directories, they will no longer be accessible after this time.
16 December 2024 | Kathleen | Completed | /old_lustre will be unmounted at or shortly after 9am. (Moved from 11 Dec). If you still have files in your old home and scratch directories, they will no longer be accessible after this time.
4-8 November 2024 | Myriad, Young | Completed | New drivers being deployed on the GPU nodes in a rolling fashion. No outage of all nodes at once.
8 October 2024 | Kathleen | Completed | There will be a brief ACFS outage while we switch the network gateway from one util node to another. If all goes well it will recover quickly, but reading and writing to the ACFS will hang while it is in progress. We will do this on 8 Oct maintenance day.
7 October 2024 | Young, Michael | Completed | Migration to new filesystem. Jobs will be drained and logins prevented for 9am. Expected to be back up the following day. Once back up, all jobs will be in `hqw` status and users will need to migrate their data from `/old_lustre/home/username` and `/old_lustre/scratch/username` which will be read-only. Users with accounts on both systems will need to log into each to see their old data, but will be copying it to the same shared home and Scratch. SSH keys will be copied to the new filesystem so login is possible. The two `/old_lustre` will remain for three months + 7 days and be removed on Tues 14 Jan 2025. [Logins enabled 8 Oct 13:30]
2-4 October 2024 | Young, Michael | Completed | Young login02 and Michael login11 will each be out of service for a day during this period for testing updates before filesystem migration. No interruption to jobs or logins to the general addresses `young.rc.ucl.ac.uk` and `michael.rc.ucl.ac.uk`.
10 September 2024 | Kathleen | Completed | Migration to new Lustre filesystem and mounting of ACFS (ARC Cluster File System) as the new backed-up location. Jobs will be drained for 8am and logins will be prevented from 9am. Expected to be back up the following day. Once back up, all jobs will be in `hqw` status and users will need to migrate their data from `/old_lustre/home/username` and `/old_lustre/scratch/username` which will be read-only. Home will no longer be backed up. `/old_lustre` will remain for three months and be removed on 11 Dec 2024.
11 June 2024 | Young | Completed | Drain of one rack for 8am to physically install new hardware. The rest of the cluster will be operating as usual. The new hardware won't be available for use for some time - will still need testing and configuring.
10 June 2024 | Michael | Completed | Full shutdown of Michael for network config alterations in preparation for new filesystem. Jobs will be drained for 8am. No logins and file access during outage. Expected to be back up later that day.
05 April -> 10 May 2024 | Young | Completed | The cabinets in Young are being re-cabled in turn. We plan for an eighth of Young to be out of service at a time, so queues may be longer due to fewer available nodes. This maintenance is essential for the longevity of Young. The rest of Young will be running jobs as usual.
9 April 2024 | Myriad | Completed | Update of module version. Jobs to be drained for 8am, modules update to be done and then jobs begun again later in the day.
5 April -> 8 April 2024 | Kathleen, Young, Michael | Completed | Annual electrical testing in datacentre. No access to cluster from 15:00 on Fri 5 April to later afternoon on Mon 8 April and no jobs will be running. We will update Singularity to Apptainer on 8th on all three clusters. On Michael we will also update the module version (brought forward from 9 April).
21 March 2024 | Kathleen, Young | Completed | Update of module version. Should not cause outage.
12 March 2024 | Myriad | Completed | Full outage to fix inaccurate quota reporting. Jobs will be drained for 8am. From 9am no one will be able to log in and no files can be accessed. We do not know until we begin quotachecks whether they will take multiple days. (Completed on Weds 13 March 13:15)
14 Nov 2023 | Myriad | Completed | Work on leaf switches in datacentre. Jobs will be drained for 8am. During the outage we intend to upgrade Singularity to Apptainer. We intend to restart jobs on the Weds 15 Nov.
29 Aug -> 1 Sept 2023 | Kathleen | Completed | Our vendors need us to do some filesystem testing on Kathleen to prepare for the replacement of its Scratch filesystem with a new filesystem later this year. We will drain jobs for 8am on Tues 29th. Depending on how long it takes to gather this information, we may not be running jobs for the rest of the week. We will update you on Thursday around lunchtime. (Jobs were restarted on Mon 4 Sept 12:00)
11 July 2023        | Myriad  | Completed | Full shutdown for replacement of both Lustre controllers in the filesystem. There will be no login access, no jobs running. Jobs will be drained for 8am. Myriad will be returned to service during the day on Weds 12 July.
10 -> 15 May 2023 | Myriad | Completed | Power work taking place in the datacentre that Myriad is in which may result in some cabinets losing power. We will be draining jobs for 8am on Weds 10 May and re-enabling them again once the work is complete, likely to be during Tues 16 May.
21 -> 23 Apr 2023 | Young, Michael, Kathleen | Completed | Electrical works in the datacentre require us to switch off all three clusters. Jobs will be drained for 12:00pm on Friday 21 April. The clusters will be powered down shortly after 12:00pm. You won't be able to log in or access data from then or throughout the weekend. We will bring the clusters back up during Monday 24 April. This may take a few hours to most of the day.
31 Jan 2023 -> 1st Feb 2023 | Myriad | Completed | Full shutdown of the system to perform firmware updates to the storage system, including all controllers and drives. Jobs will be drained for 08:00 on 31st. It is intended to make the system available again in the evening of the 1st.
23 Dec 2022 -> 3-4 Jan 2023 | Young, Michael, Kathleen | Complete | Full shutdown during UCL Christmas closing to prevent risk of filesystem damage from instability or power issues. Jobs will be drained for 9am on 23rd, shutting down at 1pm. Will be brought back up throughout 3 Jan with intent to have jobs running at some point on 4 Jan.
13 Dec 2022 -> 14 Dec 2022 | Myriad | Postponed | Upgrade of firmware on all Lustre storage disks. Jobs will be drained for 8am on Tues 13 Dec and we expect to re-enable jobs on Weds 14 Dec. Discovered that we would need to update the storage controller firmware as well, and possibly the OSSs (object store servers) and Lustre itself. We don't want to do this right before the Christmas closure so are postponing the update until later.
1 Nov 2022 -> 2 Nov 2022 | Myriad | Complete | Upgrade of firmware on all Lustre storage disks. No reading or writing of files will be possible during the upgrade interval. Jobs will be drained for 8am on Tues 1 Nov and we expect to re-enable jobs on Weds 2 Nov.
27 Sep 2022 -> 28 Sep 2022 | Myriad | Completed | **Revised dates** Moving switches. Jobs will be drained for 8am on 27 Sept and access to the login nodes disabled. Access should be restored during the day on Thurs 29. Myriad should be considered at risk for the rest of the week.
12 Apr 2022 -> 19 Apr 2022 |  Kathleen, Young, Michael | Completed | Datacentre work. Clusters will be drained of jobs and access to the login nodes disabled, for 4:00pm Tuesday 12th April. Clusters will be brought back into service during the day on Tuesday 19th April. They should be considered AT RISK that day.
1 Apr 2022 -> 4 Apr 2022 | Kathleen, Young, Michael | Completed | Datacentre work. Clusters will be will be drained of jobs and access to the login nodes disabled, for 4pm Friday 1st April. Clusters will be brought back into service on Monday the 4th April. They should be considered AT RISK that day.
7 Jan 2022 -> 10 Jan 2022 | Kathleen, Young, Michael | Completed | Datacentre power outage. Jobs will be drained for 2pm on Fri 7. Login nodes will be powered down. Clusters will be brought back online during Mon 10, should be considered at risk that day.
19 Nov 2021         | Myriad  | Completed | One of our suppliers will be onsite to replace a component. Queues are being drained.
9 Nov 2021         | Myriad  | Completed | Continued work to improve the file system. Myriad will be unavailable for the whole day. Queues are being drained for 8am.
12 Oct 2021         | Myriad  | Completed | Filesystem load tests before expansion. No jobs will be running and access to login nodes will be blocked during the test, expected to finish by end of day.
29 Sept 2021 -> 30 Sept 2021, 5 Oct 2021 | Young | Completed | Extended: expected back in service midday Tues 5 Oct. Queues drained so ZFS can be upgraded to patch a bug causing metadata server issues. No jobs will be running.
14 Sept 2021        | Myriad  | Planned | Queues drained for 8am so that the central software installs can be redistributed over more servers. This is to help mitigate the effects of current filesystem issues.
03 Aug 2021     | Young, Michael | Completed | Shortly after 8am: ~10 mins Gold outage for security updates. New job submission and Gold commands will not work during this.
12 July 2021        | Myriad  | Completed | Drain of d97a (Economics) nodes for 8am to update network config. May require less than the full day. Jobs will remain in queue until drain is over or run on other nodes if they can.
15 Jun 2021 -> 18 Jun 2021 | Myriad | Completed | Datacentre network outage. No jobs will run on the 15th, and Myriad will be at risk for the rest of the week.
14 Jun 2021 | Kathleen, Young and Data Science Platform | Completed | Datacentre network outage 8.30-9am. No login access and no jobs will run.
08 Jun 2021 | Thomas | Completed | Storage will be at risk during an essential controller reboot.
22 May 2021 -> 24 May 2021 | Michael | Completed | Datacentre network outage. Queues to be drained. Full outage, no access.
22 May 2021 -> 24 May 2021 | Young | Completed | Gold and job submission expected to be unavailable while database uncontactable due to datacentre network outage.
22 May 2021 -> 24 May 2021 | Myriad and Kathleen | Completed | Gold (i.e. high priority) job submission expected to be unavailable while database uncontactable due to datacentre network outage.
29 Mar 2021 -> 30 Mar 2021 | Myriad | Completed | A number of GPU nodes will be reserved for the NVidia AI Bootcamp on the 29th and 30th March. Some users may experience longer than usual waiting times for GPUs, from Monday the 22nd until after the event, especially for long jobs. We apologise for any inconvenience this may cause.
30 Mar 2021 | Kathleen | Completed | Essential Lustre filesystem maintenance. Queue draining starts 26th March, SSH access to login nodes disabled on the 30th. Full outage, no access.
23 Feb 2021 8-9:30am | Young | Completed | Gold and job submission expected to be unavailable for two 10-min periods while network switches upgraded.
09 Feb 2021 8-1pm   | Young   | Completed | Gold and job submission outage while we migrate our database server.
08 Jan 2021 -> 12 Jan 2021 | Kathleen | Completed | Electrical work in datacentre. Logins delayed until Tues morning while tests completed.
08 Jan 2021 -> 12 Jan 2021 | MMM Michael | Completed | Electrical work in datacentre. Logins delayed until Tues morning while tests completed.
08 Jan 2021 -> 12 Jan 2021 | MMM Young | Completed | Electrical work in datacentre. Logins delayed until Tues morning while tests completed.
02 Jul 2020 -> 09 Jul 2020 | Michael | Rollback | Lustre software upgrade to fix bug. Full outage, no access. Upgrade was unusable, downgrading to previous.
01 May 2020 -> 11 May 2020 | Myriad | Completed | Storage upgrade.
20 Mar 2020 -> 30 Mar 2020 | Myriad | Stopped, Postponed | Issues found with new storage during outage. Myriad returned to service 24th. Sync data; switch to new storage. (Important: new metadata servers). Begins midday. Full outage, no access.
16 Mar 2020 -> (was 23) 26 Mar 2020 | Michael | Completed | Firmware upgrades to bridge old and new networks. Extended to get jobs working on new nodes.
2 Mar 2020 -> 9 Mar 2020 | Michael | Completed | Installation of phase 2 hardware, network bridging
10 Feb 2020 -> 17 Feb 2020 | Myriad | Cancelled | Storage upgrade to 3PB


## Retirements

These clusters or services are being retired. Notable dates are below.

### Grace

- 1st February 2021: Job submission will be switched off. Jobs still in the queue will run. Access to the login nodes will remain for three months so you can recover your data.
- 3rd May 2021: Access to the login nodes will be removed and all data will be deleted.

### MMM Hub Thomas

This service has been retired as part of the MMM Hub. Some remains are still running
temporarily for UCL users only.

- Monday 1 March 2021: Job submission for MMM Hub users will be switched off this morning. 
Jobs already in the queue may still run. Access to the login nodes will remain for one month 
so you can retrieve data.
- Friday 5 March 2021: Queues will be drained. Any jobs left will never run.
- Thursday 1 April 2021: Access to the login nodes for MMM Hub users will be removed and all data will be deleted.

### RStudio (rstudio.rc.ucl.ac.uk)

- 16 May 2022: The old RStudio service that was at rstudio.rc.ucl.ac.uk  will be decommisioned 
and users will no longer be able to login to it.

