.. _Upgrading:

Upgrading Mothership
=======================================

Keeping all data
-----------------------
When upgrading to a new version of Mothership all configuration and runtime files are kept in two directories:

  ``config_files``
    Contains all users and passwords, any TLS certificates and configuration files.
  ``runtime_files``
    All the runtime data (images, NaCls, uplinks, logs) used by Mothership.

When stopping the Mothership make sure these two directories are kept intact.

Pull changes
------------

Start with pulling the changes from GitHub. This can be done with::

    $ git pull

or if you were using a specific ssh key::

    $ ssh-agent bash -c 'ssh-add mothership_beta.key; git pull'

Start the new Mothership
------------------------
To start Mothership with the new version use the same command as referenced in :ref:`Launch command <build_launch_mothership>`.

If Docker is in use make sure the Docker container is rebuilt before launching the new Mothership. See :ref:`mothership-in-docker`
