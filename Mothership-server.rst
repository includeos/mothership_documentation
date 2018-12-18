.. _Mothership-server:

Mothership server
=================

Configuration
-------------
All mothership configuration files should be placed in the folder: ``config_files``

Setup authentication
~~~~~~~~~~~~~~~~~~~~

If you want to run your Mothership with authentication (this is default), you need to create a .htpasswd file with one
or more users within the config_files directory:

::

    $ htpasswd -c -B config_files/.htpasswd myuser # create a .htpasswd file and add the user myuser

You will then be prompted to enter a password. Do this.

To add additional users or modify existing ones, leave out the -c option:

::

    $ htpasswd -B config_files/.htpasswd anotheruser


.. _self-signed-tls:

Create self-signed TLS certificate
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you want to run your Mothership with TLS (this is default), you need to generate a certificate and a key file in
your mothership directory:

::

    $ openssl req -x509 -newkey rsa:4096 -keyout config_files/key.pem -out config_files/cert.pem -days 365 -nodes

.. important:: This should only be used for testing, and it will require you to confirm the certificate manually in the web browser.

Start Mothership
----------------

::

    $ ./<mothership-binary> serve

If you want to start with a fresh Mothership, meaning you want to delete the images and files you created the last
time you ran your Mothership, you can add the :code:`--clean` option:

::

    $ ./<mothership-binary> serve --clean

To exit the application, press :code:`Ctrl + c`

The default settings when starting a Mothership is to start it with basic authentication and TLS enabled.

If you want to disable authentication and TLS (which you rarely want to), you can start your Motherhip with the
following command:

::

    $ ./<mothership-binary> serve --noservertls --nouplinktls --serverauth none

Then TLS will be disabled on both the API and uplink. You can choose whether you want to disable TLS on the Mothership API or the uplink (the connection between Mothership and the IncludeOS instances) or both.

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

If you want to run the Mothership in Docker but want to change some of the default settings mentioned above, you just
add :code:`serve` at the end, followed by the Mothership options you want to change or add:

::

    $ docker run --name mothership --publish 9090:9090 --publish 8080:8080 \
    -v $PWD/config_files:/home/ubuntu/mothership/config_files \
    -v mothership_storage:/home/ubuntu/mothership/runtime_files \
    -v /var/run/docker.sock:/var/run/docker.sock \
    mothership serve --verboselogging


.. _bobs-and-builders:

Bobs and Builders
-----------------
