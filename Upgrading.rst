.. _Upgrading:

Upgrading Mothership
=======================================

When a new version of Mothership is available you will have to rebuild your Mothership docker image to go to the new version.

Keeping all data intact
-----------------------
All the runtime data (images, NaCls, uplinks) used by Mothership is kept in the folder called ``runtime_files``. The :ref:`Quick-start` mounted this directory to a docker volume named ``mothership_storage``. If this volume is kept intact all the files will be used by the new version of Mothership.

The other set of data used is kept in the folder ``config_files``. This is mounted from the host and will therefore be intact as well.

Pull changes
------------

Start with pulling the changes from GitHub. This can be done with::

    $ git pull

or if you were using a specific ssh key::

    $ ssh-agent bash -c 'ssh-add mothership_beta.key; git pull'

Build new Mothership
--------------------
To build the new Mothership perform a docker build::

    $ docker build -t mothership .

Stop running Mothership
-----------------------
To stop and remove the running mothership simply perform::

    $ docker stop mothership
    $ docker rm mothership

Start the new Mothership
------------------------
To start Mothership with the new version use the same command as referenced in :ref:`Launch command <build_launch_mothership>`.
