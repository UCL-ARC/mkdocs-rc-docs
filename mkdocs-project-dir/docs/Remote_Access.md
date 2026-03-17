---
title: Remote Access
layout: docs
---

# Remote Access to Research Computing Resources

UCL's Research Computing services are accessible from inside the UCL firewall.  If you wish to connect from outside, you need to either connect through a VPN or use SSH to log in to a machine accessible from outside and use that to "jump" through into the UCL network.

As of 23 March 2026, you need to set up SSH keys on the SSH gateway to be able to log into it from outside UCL. It accepts password logins when you are on the [UCL VPN](https://www.ucl.ac.uk/isd/services/get-connected/ucl-virtual-private-network-vpn) or on [Desktop@UCL Anywhere](https://www.ucl.ac.uk/isd/services/computers/remote-access/desktopucl-anywhere) - you can use these routes to do the initial set up your SSH keys which you can then use from anywhere not at UCL.

- [SSH Gateway and Key Authentication documentation from ISD](https://liveuclac.sharepoint.com/sites/Cloud/SitePages/SSH-Gateway-+-Key-Authentication.aspx)


## Connecting to the jump boxes

You can connect to the jump boxes by connecting with your SSH client to:

```
ssh-gateway.ucl.ac.uk
```

Once connected you can then log on to the UCL RC service you are using as normal.

You can configure your ssh client to automatically connect via these jump boxes so that you make the connection in one step.

### Single-step logins using tunnelling

#### Linux / Unix / macOS

##### On the command line
```
# Log in to Kathleen, jumping via jump box
# Replace ccxxxxx with your own username.
ssh -o ProxyJump=ccxxxxx@ssh-gateway.ucl.ac.uk ccxxxxx@kathleen.rc.ucl.ac.uk
```
or
```
# Copy 'my_file', from the machine you are logged in to, into your Scratch on Kathleen
# Replace ccxxxxx with your own username.
scp -o ProxyJump=ccxxxxx@ssh-gateway.ucl.ac.uk my_file ccxxxxx@kathleen.rc.ucl.ac.uk:~/Scratch/
```

This tunnels through the jump box service in order to get you to your destination - you'll be asked for your password twice, once for each machine. You can use this to log in or to copy files.

You may also need to do this if you are trying to reach one cluster from another and there is a firewall in the way.

Useful resources can be found here:

* [SSH key pair pt 1 (moodle)](https://moodle.ucl.ac.uk/mod/page/view.php?id=4845640) (UCL users)
* [SSH key pair pt 2 (moodle)](https://moodle.ucl.ac.uk/mod/page/view.php?id=4845645) (UCL users)
* [SSH key pair pt 1 (mediacentral)](https://mediacentral.ucl.ac.uk/Play/96472) (non-UCL users)
* [SSH key pair pt 2 (mediacentral)](https://mediacentral.ucl.ac.uk/Play/96694) (non-UCL users)

##### Using a config file

You can create a config which does this without you needing to type it every time.

Inside your `~/.ssh` directory on your local machine, add the below to your `config` file (or create a file called `config` if you don't already have one).

Generically, it should be of this form where `<name>` can be anything you want to call this entry. You can use these as short-hand names when you run `ssh`.

```
Host <name>
   User <remote_user_id>
   HostName <remote_hostname>
   proxyCommand ssh -W <remote_hostname>:22 <remote_user_id>@ssh-gateway.ucl.ac.uk
```
This `proxyCommand` option causes the commands you type in your client to be forwarded on over a secure channel to the specified remote host.

On newer versions of OpenSSH, you can use `ProxyJump <remote_user_id>@ssh-gateway.ucl.ac.uk` 
instead of this `proxyCommand` line.

Here are some examples - you can have as many of these as you need in your config file.
```ssh-config
Host myriad
   User ccxxxxx
   HostName myriad.rc.ucl.ac.uk
   proxyCommand ssh -W myriad.rc.ucl.ac.uk:22 ccxxxxx@ssh-gateway.ucl.ac.uk

Host kathleen01
   User ccxxxxx
   HostName login01.kathleen.rc.ucl.ac.uk
   proxyCommand ssh -W login01.kathleen.rc.ucl.ac.uk:22 ccxxxxx@ssh-gateway.ucl.ac.uk

Host aristotle
   User ccxxxxx
   HostName aristotle.rc.ucl.ac.uk
   proxyCommand ssh -W aristotle.rc.ucl.ac.uk:22 ccxxxxx@ssh-gateway.ucl.ac.uk
```

You can now just type `ssh myriad` or `scp file1 aristotle:~` and you will go through the jump box. You'll be asked for login details twice since you're logging in to two machines, the jump box and your endpoint.  

#### Windows - WinSCP

WinSCP can also set up SSH tunnels.

 1. Create a new session as before, and tick the Advanced options box in the bottom left corner.
 2. Select Connection > Tunnel from the left pane.
 3. Tick the Connect through SSH tunnel box and enter the hostname of the gateway you are tunnelling through, for example ssh-gateway.ucl.ac.uk
 4. Fill in your username and password for that host. (Central UCL ones for the Gateway).
 5. Select Session from the left pane and fill in the hostname you want to end up on after the tunnel.
 6. Fill in your username and password for that host and set the file protocol to SCP.
 7. Save your settings with a useful name.

#### Creating a tunnel that other applications can use

Some applications do not read your SSH config file and also cannot set up tunnels themselves,
but can use one that you have created separately. FileZilla in particular is something you
may want to do this with to transfer your files directly to the clusters from outside UCL using 
a graphical client.

##### SSH tunnel creation using a terminal

You can do this in Linux, macOS and the Windows Command Prompt on Windows 10 and later.

Set up a tunnel between a port on your local computer (this is using 3333 as it is unlikely to be
in use, but you can pick different ones) to Myriad's port 22 (which is the standard port for ssh), 
going via a UCL gateway.


```
# replace ccxxxxx with your UCL username
ssh -L 3333:myriad.rc.ucl.ac.uk:22 ccxxxxx@ssh-gateway.ucl.ac.uk 
```

You may also want to use the `-N` option to tell it not to execute any remote commands and 
`-f` to put this command into the background if you want to continue to type other commands 
into the same terminal.

The tunnel now exists, and `localhost:3333` on your computer connects to Myriad.

You can do this with ports other than 22 if you are not wanting to ssh in but to instead connect
with a local browser to something running on Myriad. Here the port remains as 3333,
something could be launched on that port on Myriad and your browser could be pointed at 
`localhost:3333` to connect to it.

```
# replace ccxxxxx with your UCL username
ssh -L 3333:myriad.rc.ucl.ac.uk:3333 ccxxxxx@ssh-gateway.ucl.ac.uk
```

Do not leave things like this running for long periods on the login nodes.

##### SSH tunnel creation using PuTTY

On Windows you can also [set up a tunnel using PuTTY](https://winscp.net/eng/docs/guide_tunnel#tunnel_putty).

##### Connect to your tunnel with an application (like FileZilla)

You can then tell your application to connect to `localhost:3333` instead of Myriad. If it has 
separate boxes for hostname and port, put `localhost` as the hostname and `3333` as the port.

## File storage on the Gateway servers

The individual servers in the pool for the Gateway service have extremely limited file storage space, intentionally, and should not be used for storing files - if you need to transfer files you should use the two-step process above.  This storage should only be used for SSH configuration files.

This storage is not mirrored across the jump boxes which means if you write a file to your home directory, you will not be able to read it if you are allocated to another jump box next time you log in.

## Key management

!!! warning
    If you use SSH keys you absolutely **MUST NOT STORE UNENCRYPTED PRIVATE KEYS ON THIS OR ANY OTHER MULTI-USER COMPUTER**.  We will be running regular scans of the filesystem to identify and then block unencrypted key pairs across our services.

There are currently two servers in the pool, internally named `ejp-gateway01` and `ejp-gateway02`. 

Because the `/home` filesystem is not shared across the jump boxes, you need to sync SSH configuration files like `~/.ssh/authorized_keys` across all the available jump boxes so that the change takes effect whichever jump box you are allocated to.

You can see which machine you are logged into by the bash prompt.

So for example, if on `ejp-gateway02` then do:

```console
[ccaaxxx@ad.ucl.ac.uk@ejp-gateway02 ~]$ scp -r ~/.ssh ejp-gateway01:

Password:
known_hosts 100% 196 87.1KB/s 00:00
authorized_keys 100% 0 0.0KB/s 00:00
[ccaaxxx@ad.ucl.ac.uk@ejp-gateway02 ~]$
```

and similarly if on `ejp-gateway01` do `scp -r ~/.ssh ejp-gateway02:`
