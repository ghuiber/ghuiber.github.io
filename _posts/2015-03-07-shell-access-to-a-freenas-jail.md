---
layout: post
title: Shell access to a FreeNAS jail
tags:
- FreeNAS
---

You have in-browser shell access to your FreeNAS jail: Jails tab, click on the jail of interest, then click the Shell button. That's OK, but maybe shell access from a Mac OS X Terminal would be more pleasant.

These are my notes of how I got that working. For our purposes the server is the FreeNAS jail where you want access; the client is your Mac. The server's command line is the prompt in the browser shell (and it's a root prompt by default). The client's is the Terminal prompt. Below are the things you need to do in each:

### On the server

1. Run `adduser` to add yourself as a regular user to the jail. Might as well set the same username that you have on the client, and set a random password (copy it to your password manager). When asked `Invite <USER> into other groups? []:` type `wheel`. 
2. Run `passwd` to set a root password. This will enable your `<USER>` to `su` and gain root privileges as needed, once the ssh connection is established.
3. Edit `/etc/rc.conf` to set `sshd_enable="YES"`.
4. `cd` to `/usr/home/<USER>`, hereafter `~/`, and run `ssh-keygen`. If all goes well, this will produce a few key pairs, among them `ssh_host_dsa_key` and `ssh_host_rsa_key`. One pair is enough.
5. Copy one public key to the `authorized_keys` file like so: `cp ~/.ssh/ssh_host_dsa_key.pub ~/.ssh/authorized_keys`.
6. Start the daemon: `service sshd start`

### On the client

At this point you have password-based ssh access to the FreeNAS jail. If the user names match you can simply type `ssh 192.168.1.x` -- where the thing after `ssh` is your jail's local IPv4 address, assigned by the FreeNAS host. You will be prompted for the `<USER>`'s password, which you set to that random string that you copied to your password manager. If all is well, once you type that password you're in, and you'll find yourself at the jail's command line. Now get out with `exit`. This was just a test.

1. Copy your private key to the client's .ssh folder: `scp <USER>@192.168.1.x:/.ssh/ssh_host_dsa_key ~/.ssh`.
2. Add it with `ssh-add ~/.ssh/ssh_host_dsa_key`.

### Back on the server

1. Make one small edit to `/etc/ssh/sshd_config`: uncomment the line that says `ChallengeResponseAuthentication no`.

2. Restart the daemon: `service sshd restart`

### Back on the client

Now you should have public key access to the jail. If you type `ssh 192.168.1.x` you will no longer be prompted for a password. Instead there will be a passphrase or, if you've left it blank when you set up the key pairs, you'll be let right in.

### Why I did this

I just like the Terminal better than the browser-based shell, and I expect that it will be a nicer workflow as I interact with this jail to have two terminal windows open rather than toggle between a terminal window and the browser.

But I really started down this path when I tried to set up [`MOSH`](https://mosh.mit.edu/) access to the jail from Terminal, and I got nowhere. Then [this thread](https://forums.freenas.org/index.php?threads/install-mosh-on-freenas.25429/) convinced me that [`tmux`](http://tmux.sourceforge.net/) would be just as good for my needs.


