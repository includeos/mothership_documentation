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

.. _mothership-server-options:

Server options
~~~~~~~~~~~~~~

To provide options to mothership there are two possibilities:

1. Launch parameters to ``mothership serve``. Options use the ``--<option>`` format.
2. Options in config file, default location: ``config_files/config.yaml`` supplied in a ``key: value``.

Notable options are::

      --name string                  Name that Mothership reports back
      --certfile string              Certificate file for TLS
      --clean                        <bool, optional> clean everything
      --keyfile string               Private key file for TLS
      --serverauth string            server auth method (default "none")
      --serverport string            port number (default "8080")
      --verboselogging               <bool, optional> verbose logging
      --dockeroptions                Options to use when building in Docker inside Mothership
      --config                       Manually provided path to config file


.. _mothership-in-docker:

Running in Docker
~~~~~~~~~~~~~~~~~

Mothership comes with a Dockerfile which includes necessary dependencies.

First build and the run the Mothership Docker container::

  $ docker build -t mothership .
  $ docker run \
    --name mothership \
    -p 8080:8080 \
    -p 9090:9090 \
    -v $PWD/config_files:/home/ubuntu/mothership/config_files \
    -v mothership_storage:/home/ubuntu/mothership/runtime_files \
    -v /var/run/docker.sock:/var/run/docker.sock \
    mothership serve

The options used are::

    First are the Docker options:
        --name mothership                   Give Docker container name
        -p 8080:8080                        Forward port 8080
        -p 9090:9090                        Forward port 9090
        -v $PWD/config_files:/home/ubuntu/mothership/config_files   Bind-mount config_files folder into mothership
        -v mothership_storage:/home/ubuntu/mothership/runtime_files Mount named volume into mothership
        -v /var/run/docker.sock:/var/run/docker.sock  Mount hosts Docker process into container
    Then the mothership options:
        mothership serve                    Start mothership server

If you want to run the Mothership in Docker but want to change some of the default settings mentioned above, you just
add :code:`serve` at the end, followed by the Mothership options you want to change or add:

::

    $ docker run --name mothership --publish 9090:9090 --publish 8080:8080 \
    -v $PWD/config_files:/home/ubuntu/mothership/config_files \
    -v mothership_storage:/home/ubuntu/mothership/runtime_files \
    -v /var/run/docker.sock:/var/run/docker.sock \
    mothership serve --verboselogging

Docker in Docker
^^^^^^^^^^^^^^^^

Building IncludeOS images is performed by a Docker container. This is used to deliver a preconfigured build environment and makes it more convenient to release new IncludeOS versions. In order to allow for the Mothership to use Docker commands the servers Docker socket is mounted into the Docker container: ``-v /var/run/docker.sock:/var/run/docker.sock``.

.. note:: If you are experiencing problems with permissions for the mounted resources you might need to launch the Docker container with ``--dockeroptions "--privileged"`` as a Mothership option as well.

.. _bobs-and-builders:

Bobs and Builders
-----------------

In order to build images with specific versions of IncludeOS there was a need to create a new system. In addition there needed to be a way to manage these different Builders. Therefore the following terms have been introduced into Mothership:

.. glossary::

Builder
  A Builder is able to produce images with **one** specific version of IncludeOS. All Builders are by definition ready to be used.
  The Builder is used to perform the following two actions:

    #. Build IncludeOS images
    #. Perform NaCl validation

Bob
  A Bob is an abstraction for a resource that can become a Builder. In order for the Bob to become a Builder it needs to be prepared, this could mean it needs to be installed or downloaded.

BobProvider
  A BobProvider is a resource which provides Bobs. These Bobs can be prepared to become Builders.

Both Bobs and Builders have the following information:

    ID
      ID of the Bob/Builder. Used in all API calls when it is required to specify a Bob/Builder.
    Name
      Name of the Bob/Builder.
    Version
      The version tag that IncludeOS images built with this Bob/Builder will report as its version.
    VcsRef
      The Git commit that the IncludeOS version was built from
    BuildDate
      The date of the last Git commit.
    ProviderID
      Which provider the Bob/Builder comes from.

Usage and examples
~~~~~~~~~~~~~~~~~~

Example 1: Preparing a Builder
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In order to prepare a Builder the following tasks must be completed:

  #. Get list of available BobProviders::

      /v1/bobproviders

  #. Update one of the Bobproviders, here ``DockerHub`` is chosen::

      /v1/bobproviders/DockerHub/update

  #. Get list of available Bobs from the ``DockerHub`` provider::

      /v1/bobproviders/DockerHub/bobs

  #. Prepare a specific Bob with ID ``idNum1`` and turn it into a Builder::

      /v1/bobproviders/DockerHub/prepare/bobs/idNum1

  #. Check list of Builders::

      /v1/builders


Example 2: Building and validating NaCls
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

To perform actions with a specific Builder the ID is required.

If we wanted to build with the Builder from example 1 we would have to call::

  /v1/images/build/services/Starbase/builders/idNum1

To validate a NaCl the following endpoint would be called::

  /v1/nacls/validate/builders/idNum1
