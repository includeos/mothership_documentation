.. _Quick-start:

Quick start
===========

Dependencies
------------

============================================================================= =======================================================
Tool                                                                           Usage
============================================================================= =======================================================
`docker <https://docs.docker.com/install>`__                                   Building IncludeOS
`qemu-img <https://linux.die.net/man/1/qemu-img>`__                            Converting disk
`htpasswd <https://httpd.apache.org/docs/2.4/programs/htpasswd.html>`__        Creating users. Often provided by the *apache-utils* package
============================================================================= =======================================================

1. Clone Git repository
-----------------------
::

    $ git clone git@github.com:includeos/mothership-beta.git

To clone using a specified unique ssh key::

    $ ssh-agent bash -c 'ssh-add mothership_beta.key; git clone git@github.com:includeos/mothership-beta.git'

2. Create user
--------------
Mothership comes with basic authentication enabled by default. It is therefore necessary to create a user:
::

    $ touch config_files/.htpasswd               # Create empty .htpasswd file
    $ htpasswd -B config_files/.htpasswd myuser  # Add the user myuser

3. Configure TLS
----------------
TLS is also enabled by default. Mothership expects two files to be present in the folder ``config_files``:

:cert.pem: File containing your certificate
:key.pem: File containing your private key

For instructions on how to generate a self-signed certificate for testing see: :ref:`self-signed-tls`

.. _build_launch_mothership:

4. Launch Mothership
------------------------------
Launch Mothership with the command::

  $ ./<mothership-binary> serve

To see all launch options run ``./<mothership-binary> serve -h`` or see :ref:`mothership-server-options`.
To run Mothership inside a Docker container see :ref:`mothership-in-docker`.


This will launch the mothership server. Make sure there are no errors in the launch output and the following lines indicate that basic auth and TLS are properly configured::

    INFO[0014] 1 registered user
    INFO[0014] Setting up hangar (uplink) with TLS on :9090
    INFO[0014] Setting up server with TLS on :8080
    ⇨ https server started on [::]:8080

5. Access Mothership
--------------------
There are three ways to interact with the Mothership server.

Web GUI
    This GUI is available at: `https://localhost:8080 <https://localhost:8080>`__, for more information on this please see: :ref:`Mothership GUI<the-website>`
API
    For more information on the various API endpoints, see the API documentation at `https://localhost:8080/apidocs/apidocs.html <https://localhost:8080/apidocs/apidocs.html>`__
Command-line interface
    The binary ``mothership-linux-amd64`` used to launch the server also works as a CLI

Using the CLI the following command will give you information about the running instances::

    $ ./<mothership-binary> --username <username> --password <password> instances
    Instances:
    Alias  Status  ID  Image Tag  Launched
