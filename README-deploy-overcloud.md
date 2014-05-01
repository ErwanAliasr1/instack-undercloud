[← README](README.md)

Now that you have a working undercloud, let's deploy an overcloud.  Note that deploy-overcloud can be configured for
individual environments via environment variables. The variables you can set are documented below before the calls to
the script. For their default values, see the deploy-overcloud script itself.

## Preparing for the Overcloud

1. You '''must''' source the contents of `/root/stackrc` and `/root/tripleo-undercloud-passwords` into your shell before
running the instack-* scripts that interact with the undercloud and overcloud. In order to do that you can copy that
file to a more convenient location or use sudo to cat the file and copy/paste the lines into your shell environment.

1. Run the prepare-for-overcloud script to get setup. This script will avoid re-downloading images if they already exist
in the current working directory.  If you want to force a redownload of the images, delete them first.

    instack-prepare-for-overcloud

1. If you're testing an all VM setup, make sure you have copied the public key portion of the virtual power ssh key into
the virtual power user's ~/.ssh/authorized_keys on the virtual machine host.

1. Create a deploy-overcloudrc script to set variable values you'll need to deploy the overcloud. Note that the
variables must be exported so that their values are picked up by instack-deploy-overcloud.  Example rc files containing
example values for above variables can be found in [http://openstack.redhat.com/Instack_FAQ  Instack FAQ].  Most of the
example values will work as-is if you've been using the defaults up until now, but the MACS value will most likely need
to be altered to match your environment.


    #CPU: number of cpus on baremetal nodes
    #MEM: amount of ram on baremetal nodes, in MB
    #DISK: amount of disk on baremetal nodes, in GB
    #ARCH: architecture of baremetal nodes, amd64 or i386
    #MACS: list of MAC addresses of baremetal nodes
    #PM_IPS: list of Power Management IP addresses
    #PM_USERS: list of Power Management Users
    #PM_PASSWORDS: list of Power Management Passwords
    #NeutronPublicInterface: Overcloud management interface name
    #OVERCLOUD_LIBVIRT_TYPE: Overcloud libvirt type, qemu or kvm
    #NETWORK_CIDR: neutron network cidr
    #FLOATING_IP_START: floating ip allocation start
    #FLOATING_IP_END: floating ip allocation end
    #FLOATING_IP_CIDR: floating ip network cidr



### Scaling
To scale the Compute, Block Storage or Swift Storage nodes, you can override the default values from the
instack-deploy-overcloud scripts in your rc file.  The defaults for those scripts are:

    COMPUTESCALE=${COMPUTESCALE:-1}
    BLOCKSTORAGESCALE=${BLOCKSTORAGESCALE:-1}
    SWIFTSTORAGESCALE=${SWIFTSTORAGESCALE:-1}




NOTE: Don't forget to source deploy-overcloudrc AND source tripleo-undercloud-passwords before running the deployment
script.

## Deploying the Overcloud

1. Deploy the overcloud using provided  [https://wiki.openstack.org/wiki/Heat Heat] templates or the
[https://wiki.openstack.org/wiki/TripleO/Tuskar Tuskar] CLI . Run one of the following examples.

    # heat
    instack-deploy-overcloud

    #tuskar
    instack-deploy-overcloud-tuskarcli

  #[https://wiki.openstack.org/wiki/Tuskar/UsageGuide tuskar-ui](WIP)


After a successful deployment, you should see "Overcloud Deployed" in the standard output of the terminal.  Next steps:
[Testing the Overcloud](README-test-overcloud.md)


