# Myriad Open OnDemand Pilot

!!! note

    This component of the Myriad service is currently in limited pilot.
    
    As a pilot service, details are subject to change.

Open OnDemand (OOD) is a setup that allows you to connect to, and use, an existing HPC cluster via a web browser.

We've set this up on Myriad as a pilot service.

[Click Here To Access the OnDemand Pilot Service :octicons-link-external-16:](https://ood.myriad.rc.ucl.ac.uk/){ .md-button target="_blank" }

It currently has the following capabilities:

 - file browsing, downloading, and uploading
 - basic text editing
 - SSH access in a browser window
 - creating and running jobs based on templates
 - running interactive desktops on the compute nodes
 - running interactive Jupyter servers on the compute nodes

These may expand as we get better acquainted with this system, or add other integrated interactive components. Interactive R Studio/Posit sessions in particular seem possible but not in the scope of the pilot.

For the interactive components, we'll especially need feedback, and be looking for metrics on, whether they interfere with and are interfered with by the batch compute workloads -- it's much more useful to have an interactive desktop available 2 minutes from now than 3 hours from now.

You'll only be able to access Myriad's OnDemand servers from inside the UCL network, just like the SSH access to Myriad. If you're attempting to access these from off-campus, you'll need to use [the UCL VPN](https://www.ucl.ac.uk/isd/services/get-connected/ucl-virtual-private-network-vpn/), a SOCKS proxy, or some other mechanism of routing through the UCL firewalls.

The pilot service is not currently available to all Myriad users: there is a limited access group for it. If you need to request an addition to this group, please mail <mailto:rc-support@ucl.ac.uk>. If you are not in the group, Microsoft's authentication systems should refuse you access, and if you attempt to access any of the internal links, you should see a page saying "Unauthorized" (HTTP error 401).

## Logging in for the First Time

The first time you connect, Microsoft's authentication systems will ask whether you're happy to let our Open OnDemand instance use Microsoft 365 to authorise you. This is expected and normal.

## Tools

Ideally we'd like to keep documentation light, since this is supposed to be *relatively* discoverable, but we'll be looking for feedback on what needs explicitly describing.

### The File Browser

++"Files"++ ➡ ++"Home Directory"++ 

This should hopefully look pretty familiar if you've used, e.g. Dropbox or OneDrive's web interfaces.

From here, you can view the files on Myriad, and download or upload files. You can upload files either by clicking the ++"Upload"++ button, or by dragging them onto the window.

!!! warn

    The upload and download mechanisms here have a size limit of ~10 GiB.

<!-- ^-- this is changeable, see: https://osc.github.io/ood-documentation/latest/customizations.html#set-upload-limits -->
!!! warn

    The list of files can take a second to populate: if it shows no files at first, it's probably just being slow.


We may add additional storage entries in the menu and side bar -- for example, shared project directories -- later and as these become available.  However, these will not include storage directly attached to other clusters, for technical reasons.

### Active Jobs

++"Jobs"++ ➡ ++"Active Jobs"++

This is essentially an interface to the `qstat` command, so it shows queued and running jobs, and not completed jobs. You can also delete/cancel a queued or running job from here.

### Job Composer

++"Jobs"++ ➡ ++"Job Composer"++

### Terminal in the Browser

++"Clusters"++ ➡ ++"Myriad Shell Access"++

This lets you log in to Myriad using SSH, giving you a terminal in the browser window.

### Interactive Desktop

++"Interactive Apps"++ ➡ ++"Myriad Desktop"++

This submits a job that launches a desktop GUI on a compute node. For ease of maintenance, this is a containerised Ubuntu 24.04 LTS desktop Apptainer container, but it still has access to all the applications and the shared filesystems. It's possible having Ubuntu inside the container and RHEL outside could cause problems, so please let us know if you see problems you're not expecting.

We may replace or supplement this with other container images (e.g. RHEL or Fedora) at a later date if we think it gives a better experience.

Once the job has been configured and submitted, the site sends you to the ++"My Interactive Sessions"++ page, where you can Cancel it, or open the remote desktop viewer.

If you close the remote desktop viewer, the job will stay running and you can reconnect later. If you attempt to log out in the remote desktop viewer, the job will end.

Please note that the Applications menu in the interactive desktop does not currently have icons or entries for any research applications -- these still have to be run from the command line, but can still present a graphical window in the remote desktop.

### Jupyter

++"Interactive Apps"++ ➡ ++"Jupyter Notebook"++

Similarly to the Interactive Desktop, this can submit a job that launches a Jupyter Notebook session on a compute node. It then acts as a proxy to let you access that server from your browser.

This uses a Python virtualenv with only the packages required to run Jupyter -- you may wish to install additional kernels from the command line.

