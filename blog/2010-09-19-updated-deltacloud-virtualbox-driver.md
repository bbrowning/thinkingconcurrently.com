---
layout: blog
title: Updated Deltacloud VirtualBox Driver
---

The [SteamCannon][] project uses [Deltacloud][] to abstract away the
details of specific cloud providers when launching instances. For
local development, it's nice to be able to launch a cluster of virtual
machines in [VirtualBox][] instead of actually starting and stopping
instances on a public cloud like Amazon EC2.

[steamcannon]: http://steamcannon.org
[deltacloud]: http://deltacloud.org
[virtualbox]: http://www.virtualbox.org

Michal Fojtik created a [deltacloud-virtualbox-driver][] many months
ago to add VirtualBox support to Deltacloud. However, Deltacloud has
evolved quite a bit since it was initially released and that driver no
longer works with the current implementation.

[deltacloud-virtualbox-driver]: http://gitorious.org/deltacloud-devel/deltacloud-virtualbox-driver

I've taken Michal's great start and updated it to work with the latest
Deltacloud release, giving it a few other changes in the process.

* Switched to the excellent [virtualbox gem][] which uses the
  VirtualBox COM API instead of shelling out to the VBoxManage
  process. The COM API is substantially faster and should allow this
  driver to work on Windows instead of just Linux/Mac (but it hasn't
  been tested on Windows).

* Networking settings of the launched instance are now copied from the
  parent image instead of being hardcoded to a specific value.

* The driver now works under JRuby. There are still some optimizations
  to be made in the code based on which environment it's running in,
  but it will at least run in JRuby and MRI.


[virtualbox gem]: http://github.com/mitchellh/virtualbox

### Installing the VirtualBox Driver

The driver is very new and still needs to mature a bit before I submit
it to the upstream Deltacloud project. Until then, if you're
comfortable living on the edge, read below for installation
instructions.

* Install VirtualBox 3.2.x (might work with 3.1.x but has not been
  tested) - <http://www.virtualbox.org/wiki/Downloads>

* Install bbrowning-virtualbox gem (temporary fork of upstream
  implementing some missing features)

        $ gem install bbrowning-virtualbox --pre

* Install bbrowning-deltacloud-core gem

        $ gem install bbrowning-deltacloud-core

* Launch deltacloud-core with the VirtualBox driver

        $ deltacloudd -i virtualbox

If all goes well, you should be able to go to <http://localhost:3001>
in your browser and see the Deltacloud web interface. Clicking the
Images link should display all virtual machines you have setup inside
VirtualBox - if you don't see any, try creating a new virtual machine
in VirtualBox and then come back to play around.

The instances launched from an image clone the hard drive of the image
but any changes made in the instance are not reflected in the parent
image. If you don't have a lot of free hard drive space, make sure to
go to the Instances page and destroy stopped instances that you no
longer need.

Enjoy!
