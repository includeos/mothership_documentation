.. _Mothership-server-for-developers:

Mothership server
=================

Start Mothership
----------------

Locally
~~~~~~~

::

    $ cd $GOPATH/src/github.com/includeos/mothership
    $ ./mothership serve

If you want to start with a fresh Mothership, meaning you want to delete the images and files you created the last
time you ran your Mothership, you can add the :code:`--clean` option:

::

    $ ./mothership serve --clean

Images are built using Docker.

To exit the application, press :code:`Ctrl + c`

The default settings when starting a Mothership is to start it with basic authentication and TLS enabled.

If you want to disable authentication and TLS (which you rarely want to), you can start your Motherhip with the
following command:

::

    $ ./mothership serve --noservertls --nouplinktls --serverauth none

Then TLS will be disabled on both the API and uplink. You can choose whether you want to disable TLS on the Mothership API or the uplink (the connection between Mothership and the IncludeOS instances) or both.

.. note:: If you want to disable authentication and TLS, you also need to update a constant in the mothership_client so that a Login-button will not be displayed when you visit the Mothership website:

    ::

        $ cd mothership_client
        $ <open the file components/common/clientSettings.js>
        $ <set the constant enableLogin to false>
        $ ./copyfiles.sh

In a Docker container
~~~~~~~~~~~~~~~~~~~~~

::

    $ cd $GOPATH/src/github.com/includeos/mothership

If you want Docker to handle everything for you, from the mothership_client to the IncludeOS installation, you can
do the following:

::

    $ make mothership
    $ docker run --name mothership --publish 9090:9090 --publish 8080:8080 \
    -v $PWD/config_files:/home/ubuntu/mothership/config_files \
    -v mothership_storage:/home/ubuntu/mothership/runtime_files \
    -v /var/run/docker.sock:/var/run/docker.sock \
    mothership

Exit the container by pressing :code:`Ctrl + c`.

If you want to run the Mothership in Docker but want to change some of the default settings mentioned above, you just
add :code:`serve` at the end, followed by the Mothership options you want to change or add:

::

    $ docker run --name mothership --publish 9090:9090 --publish 8080:8080 \
    -v $PWD/config_files:/home/ubuntu/mothership/config_files \
    -v mothership_storage:/home/ubuntu/mothership/runtime_files \
    -v /var/run/docker.sock:/var/run/docker.sock \
    mothership serve --verboselogging

If you get a Conflict message when starting your Mothership in Docker, remove the previously created container:

::

    $ docker rm mothership

Stop your Mothership container:

::

    $ docker stop mothership # `docker kill mothership` is also an option if this doesn't work

List your running Docker containers:

::

    $ docker ps

List all your Docker containers:

::

    $ docker ps -a

Clean up your Docker environment:

::

    $ docker system prune
    $ <answer y when asked if you want to continue>

Server options
~~~~~~~~~~~~~~

To provide options to mothership there are two possibilities:

1. Launch parameters to ``mothership serve``. Options use the ``--<option>`` format.
2. Options in config file: ``config_files/config.yaml`` supplied in a ``key: value``.

Notable options are::

      --certfile string              Certificate file for TLS
      --clean                        <bool, optional> clean everything
      --keyfile string               Private key file for TLS
      --serverauth string            server auth method (default "none")
      --serverport string            port number (default "8080")
      --verboselogging               <bool, optional> verbose logging
