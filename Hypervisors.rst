.. _Hypervisors:

Hypervisors
===========

Mothership has built in support for launching a binary on multiple hypervisors. To do this it has dependencies on tools
for each particular hypervisor.

Vcloud
------

Dependencies
~~~~~~~~~~~~

- Install `ovftool <https://www.vmware.com/support/developer/ovf/>`__ (Requires a vmware account)
- Install `Docker <https://docs.docker.com/install/>`__ (Used to add grub bootloader)

Environment variables
~~~~~~~~~~~~~~~~~~~~~

The following environment variables will have to be set in order for Vcloud to function.

:ovfToolLocation: Location of the ovftool
:vcloudUsername: Username for vcloud
:vcloudPassword: Password for vcloud

Launch options
~~~~~~~~~~~~~~

Mothership needs the following parameters set for launch
::

    $ mothership launch --hypervisor vcloud


--vcloud-net      Name of the network in vcloud to connect to
--vcloud-address  Address to vcloud
--vcloud-org      Name of vcloud organization
--vcloud-vapp     Name of vapp to generate (overwrites existing vapps with same name)


Launch Command
~~~~~~~~~~~~~~

The final launch command will look like this:
::

    $ mothership launch --hypervisor vcloud \
          --vcloud-net <name> \
          --vcloud-address <address> \
          --vcloud-org <name> \
          --vcloud-vapp <name> \
          <elf-binary-to-boot>

Multiple network interfaces
~~~~~~~~~~~~~~~~~~~~~~~~~~~

To have multiple network interfaces in your application create a ``vm.json`` in the same folder that ``launch`` is called from.

The following ``vm.json`` creates 3 interfaces:

::

    {
      "net" : [
        {"device" : "vmxnet3", "mac" : "c0:01:0a:00:00:2a"},
        {"device" : "vmxnet3", "mac" : "c0:01:0a:00:00:2b"},
        {"device" : "vmxnet3", "mac" : "c0:01:0a:00:00:2c"}
      ]
    }
