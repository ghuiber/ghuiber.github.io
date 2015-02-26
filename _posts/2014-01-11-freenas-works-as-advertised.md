---
layout: post
title: FreeNAS works as advertised
categories:
- FreeBSD
---
I decided to replace the HDD with a SSD in my Mac for Christmas, but I only got as far as buying [the thing](http://www.amazon.com/Samsung-Electronics-EVO-Series-2-5-Inch-MZ-7TE250BW/dp/B00E3W1726) and backing up the computer using Time Machine as explained [here](http://doc.freenas.org/index.php/Apple_(AFP)_Shares) to a poor man's [FreeNAS](http://www.freenas.org/) server that I cobbled together from a USB stick (for the OS) and the old [Fedora 14 home server](http://www.intel.com/content/www/us/en/motherboards/desktop-motherboards/desktop-board-di510mo.html) whose sole 500G HDD is now one big ZFS volume, with 2G of RAM. 

That's right, ZFS on one HDD with 2G of RAM. I'm not saying that this is a good setup. The official [hardware recommendation](http://doc.freenas.org/index.php/Hardware_Recommendations) is 8G for ZFS. But this is the kit I had lying around, and I just wanted to move on with the actual disk replacement; my D510MO board won't even support more than 4G of RAM (though I'm not sure why, since it was made to accommodate a [64-bit CPU](http://ark.intel.com/products/43098/Intel-Atom-Processor-D510-1M-Cache-1_66-GHz)). Anyway, I managed to make one first complete Time Machine backup and a few incremental ones before leaving for work on Monday, January 6.

I flew back on Thursday, January 9, and found a non-responsive Mac with a HDD so sick that an erase-and-install OS restore was in order. That's what you end up having to do when upon entering your password at boot-up you see the apple logo for a while, then that "prohibited access" barred circle, while the gear animation is spinning and spinning. 
I have no idea how this happened. I felt very fortunate for having made that backup. I decided that the accident was a good excuse to just [proceed with the SSD installation](http://www.ifixit.com/Guide/MacBook+Pro+13-Inch+Unibody+Early+2011+Hard+Drive+Replacement/5119) already.

The proof of this particular pudding was going to be in restoring the old system from that Time Machine backup, over the LAN, off the grossly inadequate NAS box. I am happy to report that the restore succeeded, and my Mac is back in business, now with a SSD. 
What I'm saying is this: if you don't have a [Time Capsule](http://www.apple.com/airport-time-capsule) but do have some idle hardware, FreeNAS may be a good Time Machine backup solution for you too. 

One thing you will want to know about is user quotas: a 500G NAS HDD will fill up quickly if you let Time Machine have its way with it. The solution is to set some reasonable user quotas for people in your house who might use the FreeNAS box as their Time Machine backup destination. You can do that from the web GUI. The Advanced Mode of the Create ZFS Dataset menu under Storage (or, for an existing dataset, the Advanced Mode of Edit ZFS Options) lets you set quotas four different ways; for specifics, google thin and thick provisioning. This seems to be advanced sysadmin stuff.

There is also a command-line recipe for setting user quotas [here](http://forums.freenas.org/threads/disk-quota-per-user.11511/). You get to the FreeNAS shell from the web GUI: look at the bottom of the vertical navigation menu on the left.

Smaller quotas will force Time Machine to keep a shorter history. It deletes old backups as it runs out of space -- so, less room, shorter history. That is not a bad thing.
