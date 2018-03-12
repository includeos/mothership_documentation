.. _Using-mothership:

Using Mothership
================

Your first IncludeOS instance
-----------------------------

1. Create a configuration for your instance
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Go to the NaCl page**

NaCl is a configuration language for IncludeOS. On the NaCl page you can find links to the language documentation and
examples.

.. image:: _static/images/nacl.png

**Click on the Create new-button**

.. image:: _static/images/nacl-create.png

**Click on the code snippet named Iface**

This will paste the NaCl you need for configuring an instance with one interface. Change the Iface values to fit your
need and give your NaCl file a name.

.. image:: _static/images/nacl-create-with-content.png

**Click on the Validate-button to validate the NaCl content**

If there are any errors in your NaCl configuration, an error icon will be displayed in the left margin of the line
containing the error. When hovering this icon, the reason for the error will be displayed:

.. image:: _static/images/nacl-create-with-error.png

**Click on the Save-button**

The newly created NaCl file is displayed in the list if the NaCl content doesn't contain any errors:

.. image:: _static/images/nacl-with-content.png

2. Build your image
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If you are running Mothership with the native builder, you need to have IncludeOS installed on your machine. The
IncludeOS documentation can be found `here <https://includeos.readthedocs.io>`__.

**Go to the Build page**

Here you will see that your newly created NaCl is displayed in the drop-down list:

.. image:: _static/images/build.png

When building an image, information about the URL of the Mothership to connect to must be given. This is what we call
an uplink. The default uplink that is created when starting a Mothership is set to be the IP of your Mothership and
port 9090. You can view and edit the default uplink settings to your Mothership on the Settings page. Here you can also
create other uplinks.

.. image:: _static/images/settings-uplinks-default.png

On the Build page you are asked if you want to use a custom uplink for this build. If the default uplink on the
Settings page is the uplink you want to use, you don't need to check this. If you have another uplink you would like to
use, however, check the box and a drop-down list containing the names of all your uplinks will be displayed. Choose one
of these.

**Click on the Build button**

The status of the build is displayed on the right-hand side, and if everything went well, the status :code:`deployable`
will be seen at the bottom.

.. image:: _static/images/build-finished.png

**Download the image**

You can also see that the image's checksum is displayed on the Build page after a successful build, and if you click
on this you will download the image.

For more information about the image you have built, go to the Images page. All your built images will be displayed
here and you can also upload images that you have built without using Mothership.

.. image:: _static/images/images-with-content.png

Click on an image's More-button for more information about the image:

.. image:: _static/images/images-more.png

3. Boot
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

After you have downloaded the image, you can launch it on your preferred hypervisor (the
:code:`./mothership launch --hypervisor <hypervisor> <ELF-binary>` command is useful here - see our
:ref:`hypervisors` documentation), or you can boot it up locally:

.. ip forwarding on if uplink 192...., else 10.0.0.1 f.ex.
.. vm.json (net devices, specify uuid if booting locally on mac f.ex.)

::

    $ boot Starbase-nacl-0 --create-bridge

.. image:: _static/images/boot-first-instance.png

On the Instances page, you will see your instance when it has connected to the Mothership successfully:

.. image:: _static/images/instances-with-content.png

**Note:**

IP forwarding must be turned on for your instances to connect to public IP addresses.
To make the instance above (with address 10.0.0.45) connect to the Mothership on 192.168.0.13 f.ex., IP forwarding
must be turned on.
In addition, a bridge must be set up so that the instance with IP 10.0.0.45 can reach other networks.
The :code:`--create-bridge` option given to the boot command above sets up a bridge for you, but to enable IP
forwarding you have to run the following commands (on macOS):

::

    $ echo "nat on en0 from bridge43:network to any -> (en0)" > includeos-nat.conf
    $ sudo sysctl -w net.inet.ip.forwarding=1
    $ sudo pfctl -F all // flush all active rules
    $ sudo pfctl -f includeos-nat.conf -e // enable pf

An alternative to turning on IP forwarding in this case however (with a Mothership running on localhost), is to
create an uplink with URL 10.0.0.1:9090 and build an image with that. Then you only need to run
:code:`boot Starbase-nacl-0 --create-bridge`, and the instance will connect to the Mothership.
You only need to add the :code:`--create-bridge` option once.


Update your IncludeOS instance
------------------------------
