[← README](README.md)

This page includes tips, fixes and debugging steps for use with instack-undercloud:

## What do I do if I encounter an error relating to "disk is in use"?
If the undercloud machine was installed using LVM, when deploying overcloud nodes, you may see an error related to the
disk being "in use".  The workaround for this error is to:
<pre># Modify /etc/lvm/lvm.conf to set use_lvmetad to be 0
vi /etc/lvm/lvm.conf
use_lvmetad=0
# Disable and stop relevant services
systemctl stop lvm2-lvmetad
systemctl stop lvm2-lvmetad.socket
systemctl disable lvm2-lvmetad.socket
systemctl stop lvm2-lvmetad</pre>

## Are there any example rc files for overcloud deployment?
<p>The following are example rc files to source before deploying the overcloud. </p>

    #!/bin/bash
    export CPU=1
    export MEM=2048
    export DISK=30
    export ARCH=amd64
    export NeutronPublicInterface=eth0
    export OVERCLOUD_LIBVIRT_TYPE=qemu
    export NETWORK_CIDR=10.0.0.0/8
    export FLOATING_IP_START=192.0.2.45
    export FLOATING_IP_END=192.0.2.64
    export FLOATING_IP_CIDR=192.0.2.0/24
    export MACS="52:54:00:01:f4:00 52:54:00:01:f4:01 52:54:00:01:f4:02 52:54:00:01:f4:03"
    export COMPUTESCALE=1
    export BLOCKSTORAGESCALE=1
    export SWIFTSTORAGESCALE=1

<p>Example rc file for deploying the overcloud on a barmetal machine setup:</p>

    #!/bin/bash
    export CPU=1
    export MEM=2048
    export DISK=30
    export ARCH=amd64
    export MACS="52:54:00:01:f4:00 52:54:00:01:f4:01 52:54:00:01:f4:02 52:54:00:01:f4:03"
    export PM_IPS="10.10.0.01 10.10.0.02 10.10.0.03 10.10.0.04"
    export PM_USERS="username username username username"
    export PM_PASSWORDS="password password password password"
    export NeutronPublicInterface=em2
    export OVERCLOUD_LIBVIRT_TYPE=kvm
    export NETWORK_CIDR="10.0.0.0/8"
    export FLOATING_IP_START="172.17.0.45"
    export FLOATING_IP_END="172.17.0.64"
    export FLOATING_IP_CIDR="172.17.0.0/16"
    export COMPUTESCALE=1
    export BLOCKSTORAGESCALE=1
    export SWIFTSTORAGESCALE=1


## How do deploy my overcloud with new images?

To deploy new overcloud images, you'll need a clean working directory.  Either delete or move the files, or use a different
working directory.  You can either build new images locally using [instack-build-images](README-build-images.md) or download new images using
<code>instack-prepare-for-overcloud</code>.  The <code>instack-prepare-for-overcloud</code> script will delete images from
glance if they are already present.  Continue at Step 3. in [Deploying an Overcloud](README-deploy-overcloud).

## How do I delete an overcloud?

If you want to delete an overcloud and reset the environment to a state where you can deploy another overcloud, use the
instack-delete-overcloud* scripts from this repository at [Scripts]( https://github.com/agroup/instack-undercloud/tree/master/scripts).
Run one of the following examples that matches how you deployed the overcloud.

    #heat
    instack-delete-overcloud

    #tuskar
    instack-delete-overcloud-tuskarcli

## How do I redeploy my overcloud that I deployed?

First you will need to delete your overcloud using the appropriate method above (heat or tuskar).  Once the overcloud
and baremetal nodes have been deleted, you can continue to Step 5. in [Deploying an Overcloud](README-deploy-overcloud).

