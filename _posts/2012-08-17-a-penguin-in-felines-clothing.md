---
layout: post
title: A Penguin in Feline's Clothing
categories: linux mac misc
---

Having [received my new hard drives](/2012/08/16/hard-disc-delights/),
I'm in the process of setting my fileserver up from scratch again, so
I thought that I might as well take the opportunity to write a bit
about its setup!

I have a headless desktop computer running Ubuntu Server 12.04, it's
kitted out with four 2TB hard drives, split into a small RAID1 for the
root partition and a RAID5 to keep my data on. I wanted to be able to
access the data from Linux, Mac OS X and Windows; getting Linux
working with it is easy enough, so I won't bother going into that, and
Windows just requires setting up Samba, but OS X is the more
interesting one.


File Sharing From Linux to OS X
-------------------------------

(**NOTE:** This has only been tested on Lion and Mountain Lion, and
only with Ubuntu Server 11.10 and 12.04 (There are [much more detailed guides
 around](http://kremalicious.com/ubuntu-as-mac-file-server-and-time-machine-volume/),
 but some of them are outdated or wrong in places; it seemed worth
 posting a this-works-as-of-now kind of thing))

There are two main components to getting this up and running:
[Netatalk](http://en.wikipedia.org/wiki/Netatalk) and
[Avahi](http://en.wikipedia.org/wiki/Avahi).

### Netatalk

Netatalk is an open-source implementation of
[AFP](http://en.wikipedia.org/wiki/Apple_Filing_Protocol), allowing
Linux machines to serve files to Apple machines securely (not to
mention quickly). It also takes care of the fact that the directory
you're serving is not on an HFS+ formatted volume, maintaining the
metadata databases required for seamless functionality with OS X.

Netatalk is also capable of presenting volumes that are compatible
with Time Machine, although it isn't flawless and shouldn't be relied
on completely. See the Time Machine section below for more details.

It can be installed with trivially:

    $ sudo apt-get install netatalk

There are two configuration files that need to be edited to get
Netatalk up & running, `/etc/netatalk/afpd.conf` and
`/etc/netatalk/AppleVolumes.default`.

#### afpd.conf

    $ sudo nano /etc/netatalk/afpd.conf

At the bottom of the file add the following:

    - -transall -uamlist uams_dhx2.so,uams_dhx.so -nosavepassword -advertise_ssh

#### AppleVolumes.default

    $ sudo nano /etc/netatalk/AppleVolumes.default

This is the file where the directories you want available are
listed. So add lines like this for each directory:

    /mounts/media   "Media" cnidscheme:dbd options:usedots,upriv

### Avahi

Avahi advertises the service over mDNS, so that it presents itself to
other machines on the local network. On OS X, it shows up in Finder
under the Shared section. Avahi is not strictly required to
communicate with an OS X machine, however without it you have to
connect manually using the "Connect to Server" option in Finder; using
Avahi makes for a much more seamless user experience. (If you're going
to do something, do it right, right?)

    $ sudo apt-get install avahi-daemon

There are also two files to edit for this: `/etc/nsswitch.conf` &
`/etc/avahi/services/afpd.service`. afpd.service will probably not exist
by default, so you may need to create it.

#### nsswitch.conf

    $ sudo nano /etc/nsswitch.conf

Find the line beginning with `hosts:` and change it so that it matches

    hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4

#### afpd.service

    $ sudo nano /etc/avahi/services/afpd.service

Add the following:

{% highlight xml %}
<?xml version="1.0" standalone='no'?><!--*-nxml-*-->
<!DOCTYPE service-group SYSTEM "avahi-service.dtd">
<service-group>
        <name replace-wildcards="yes">%h</name>
        <service>
                <type>_afpovertcp._tcp</type>
                <port>548</port>
        </service>
        <service>
                <type>_device-info._tcp</type>
                <port>0</port>
                <txt-record>model=Xserve</txt-record>
        </service>
</service-group>
{% endhighlight %}

This advertises the machine so that it appears in Finder. The second
`<service>` entry decribes to OS X what type of machine it's
connecting to, in this case I've specified Xserve so that it appears
as a server machine.

### Finishing Up

    $ sudo /etc/init.d/netatalk restart
    $ sudo /etc/init.d/avahi-daemon restart

You should now be able to see your machine in Finder, when asked for a
username & password you should enter the details of a user on that
machine.

### Time Machine

Enabling Time Machine support is fairly simple once you have all of
the able up and running. On the server side, you simply need to add
`tm` as an option on the line for the appropriate volume in
AppleVolumes.default.

Whilst this enables support for it, it's not enough to convince a Mac
to use it because it's not formatted as HFS+. To solve this problem,
fire up a terminal on the OS X machine and enter the following:

    $ defaults write com.apple.systempreferences TMShowUnsupportedNetworkVolumes 1

With this enabled, OS X will allow you to use unsupported volumes by
creating a sparsebundle on the volume and formatting this as HFS+, so
that it can work properly.

**Warning**: On my old setup, I did encounter a few problems with Time
  Machine, although I can't determine whether it was because of my
  problems with the hard drives or not. I received the message "Time
  Machine completed a verification of your backups. To improve
  reliability, Time Machine must create a new backup for you.", which
  could be solved using [these
  instructions](http://www.garth.org/archives/2011,08,27,169,fix-time-machine-sparsebundle-nas-based-backup-errors.html). Choosing
  not to start a new backup didn't render the existing one useless, it
  could still be accessed and restored from; so no data was (as far as
  I could tell) actually lost, but it still was an annoyance to have
  to fix.

I will update this post (or make a new one) if I encounter this
problem again, and/or discover a way to solve it.

Enjoy!
