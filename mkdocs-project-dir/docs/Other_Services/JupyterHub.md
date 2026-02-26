---
title: JupyterHub
categories:
 - JupyterHub
layout: docs
---

# JupyterHub

## Overview

The Research Computing team provide a JupyterHub service
([jupyter.org](https://jupyter.org/hub)) primarily to support
teaching and is part of the Data
Science Platform. It uses the central UNIX filestore for user data. 

## Access

To access the service go to the following URL:

<https://jupyter.data-science.rc.ucl.ac.uk/>

and login with your UCL userid and password.

The Data Science Platform and all of its components are only
accessible from the UCL network. When off campus, you will need to
connect to the
[UCL VPN](https://ucl.ac.uk/isd/services/get-connected/ucl-virtual-private-network-vpn)
first.

**Please note:** The main aim of this service is to support teaching
  and the service should not be used for computationally intensive
  research. If your use starts to affect the experience of other
  users, we reserve the right to terminate sessions without
  notice. For computationally intensive
  research you should be using the [Myriad Cluster](https://www.rc.ucl.ac.uk/docs/Clusters/Myriad/).

## Troubleshooting

As mentioned above, the Data Science Platform's Jupyterhub service is only available from the UCL network. If you aren't able to see the login page, please start by checking your network status (particularly your VPN if off-campus). If you run into issues with Jupyterhub after logging in, please see below for guidance.

### 500: Internal Server Error

If you see `500: Internal Server Error` after attempting to open a notebook or new kernel, this is usually a result of configurations you have made to your local pip/conda/jupyter directories, which end up conflicting with what JupyterHub uses for sessions.
 
You can try to fix this by temporarily renaming the hidden directories in your home folder with the following steps.

1. Sign in to jupyterhub and open a terminal with `New` > `Terminal`
2. You can list all files and folders in your home directory (including hidden folders, which start with `.`) using the following command:
   - `ls -al ~/`
3. One at a time, rename/move the following directories using the `mv` command and check if you still get the error:
   - `mv ~/.local ~/.local.old`
   - `mv ~/.conda ~/.conda.old`
   - `mv ~/.jupyter ~/.jupyter.old`
4. Your home folder may have additional hidden folders (e.g. `.ipython` or `.cache`), so try renaming these too if the above don't work

This process is moving rather than deleting the directories, so you can still check on their contents, or change each back to the original name if not the source of the issue.


