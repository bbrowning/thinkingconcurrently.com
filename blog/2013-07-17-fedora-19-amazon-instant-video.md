---
layout: blog
title: 'Fedora 19 and Amazon Instant Video'
---

Amazon Instant Video and other DRM-laden flash websites don't work out
of the box with flash on Fedora 19. Below is the list of steps I took
to get this working in case it helps anyone else. The majority of
these instructions were pieced together from the post and comments at
<http://wtogami.blogspot.com/2012/12/amazon-instant-video-on-fedora-16.html>. The
linked RPMs were originally taken from that post and recompiled for
Fedora 19. These steps have only been tested in Firefox and 64 bit
Fedora 19 - I don't think they'll work for Chrome.

First, head to <http://get.adobe.com/flashplayer/> and download the
"YUM for Linux (YUM)" RPM. Then:

    # Install Adobe Flash and its browser plugin
    sudo yum install -y adobe-release-x86_64-1.0-1.noarch.rpm
    sudo yum install -y flash-plugin
    
    # Install a simple SELinux policy file for Flash
    sudo yum install -y policycoreutils-devel
    wget http://togami.com/~warren/archive/2012/adobedrm.te
    checkmodule -M -m -o adobedrm.mod adobedrm.te
    sudo semodule_package -o adobedrm.pp -m adobedrm.mod

Download the fakehal RPMs for Fedora 19 at
<http://thinkingconcurrently.com/f19_flash/fakehal-0.5.14-7.fc19.x86_64.rpm>
and
<http://thinkingconcurrently.com/f19_flash/fakehal-libs-0.5.14-7.fc19.x86_64.rpm>

    # Install the fakehal RPMs
    sudo yum install -y fakehal-0.5.14-7.fc19.x86_64.rpm \
      fakehal-libs-0.5.14-7.fc19.x86_64.rpm

At this point, make sure all Firefox windows are closed.

    rm -rf ~/.adobe/Flash_Player/
    sudo mkdir -p /etc/hal/fdi/preprobe /etc/hal/fdi/information \
      /etc/hal/fdi/policy /usr/share/hal/fdi/preprobe \
      /usr/share/hal/fdi/policy/20thirdparty
    sudo touch /var/cache/hald/fdi-cache
    sudo systemctl start haldaemon.service

Now start up Firefox and watch your favorite Amazon Instant Video. Let me know if it works for you!
