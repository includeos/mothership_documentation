.. _Getting started:

Getting started
===============

Installation
------------

- Install `Go 1.10 <https://golang.org/dl/>`__
- Install `Docker <https://docs.docker.com/install/>`__
- Install `dep <https://golang.github.io/dep/>`__ (:code:`brew install dep` on macOS)
- Install `htpasswd <https://httpd.apache.org/docs/2.4/programs/htpasswd.html>`__ if you want to run your Mothership with authentication

::

    $ cd $GOPATH
    $ git clone git@github.com:includeos/mothership.git
    $ dep ensure
    $ go build mothership.go

Setup authentication
~~~~~~~~~~~~~~~~~~~~

If you want to run your Mothership with authentication (this is default), you need to create a .htpasswd file with one or more users:

::

    $ htpasswd -c -B .htpasswd myuser // create a .htpasswd file and add the user myuser

You will then be prompted to enter a password. Do this.

To add additional users or modify existing ones, leave out the -c option:

::

    $ htpasswd -B .htpasswd anotheruser

Setup TLS
~~~~~~~~~

If you want to run your Mothership with TLS (this is default), you need to generate a certificate and a key file in your mothership folder:

::

    $ openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes

The Mothership GUI client
~~~~~~~~~~~~~~~~~~~~~~~~~

::

    $ git clone git@github.com:includeos/mothership_client.git // f.ex. in your HOME folder

MacOS

::

    $ brew install node
    $ brew install npm
    $ npm install -g webpack@2.6.1
    $ npm install
    $ ./copyfiles.sh

Ubuntu

::

    $ sudo apt install npm
    $ sudo npm install -g n // webpack needs the node command as opposed to nodejs. The npm n tool should fix that.
    $ sudo n stable
    $ sudo npm install -g webpack@2.6.1
    $ sudo npm install
    $ ./copyfiles.sh

Start your Mothership
---------------------

::

    $ cd $GOPATH/mothership
    $ ./mothership serve

If you want to start with a fresh Mothership, meaning you want to delete the images and files you created the last time you ran your Mothership, you can add the :code:`--clean` option:

::

    $ ./mothership serve --clean

By default the Mothership is also started with the native builder, meaning to build images with your local IncludeOS installation.

If you want to build your images using Docker, you can add the :code:`--builder docker` option:

::

    $ ./mothership serve --builder docker

To exit the application, press :code:`Ctrl + c`

The default settings when starting a Mothership is to start it with basic authentication and TLS enabled.

If you want to disable authentication and TLS (which you rarely want to), you can start your Motherhip with the following command:

::

    $ ./mothership serve --noservertls --serverauth none

**Note**: If you want to disable authentication and TLS, you also need to update a constant in the mothership_client so that a Login-button will not be displayed when you visit the Mothership website:

::

    $ cd mothership_client
    $ <open the file components/common.js>
    $ <set the constant enableLogin to false>
    $ ./copyfiles.sh

Start your Mothership in a Docker container
-------------------------------------------

::

    $ cd $GOPATH/mothership

If you want Docker to handle everything for you, from the mothership_client to the IncludeOS installation, you can do the following:

::

    $ ./build_mothership.sh // instead of `go build mothership.go`
    $ docker run --privileged --detach --name mothership --rm --publish 9090:9090 --publish 8080:8080 mothership

You can exclude the :code:`--detach` flag if you want to see the Mothership log output. Then you exit the container by pressing :code:`Ctrl + c`

If you want to run the Mothership in Docker but want to change some of the default settings mentioned above, you just add :code:`serve` at the end, followed by the Mothership options you want to change:

::

    $ docker run --privileged --detach --name mothership --rm --publish 9090:9090 --publish 8080:8080 mothership serve --builder docker

If you get a Conflict message when starting your Mothership in Docker, a previously started mothership container may not have exited properly. If so, you can remove it by:

::

    $ docker rm mothership

Stop your Mothership container:

::

    $ docker stop mothership // `docker kill mothership` is also an option if this doesn't work

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

The website
-----------

When you have come this far, or if you want to connect to a public Mothership that is already running, you can open your browser and go to the Mothership's website.

If you are running a Mothership locally, you will find your Mothership's website by going to `https://localhost:8080 <https://localhost:8080>`__ if you have started your Mothership with TLS enabled.

If you have started your Mothership without TLS, you will find the website at `http://localhost:8080 <http://localhost:8080>`__.

Here, if the Mothership was started with authentication enabled, you will see a Login-button:

.. image:: _static/images/login-button.png

When clicking on this, a popup will appear, asking you to fill in your username and password:

.. image:: _static/images/login.png

This username and password must match one of the entries in the previously created mothership/.htpasswd file.

If no authentication is required, you will be sent straight to the Instances page:

.. image:: _static/images/instances-start.png

Build and start your first IncludeOS instance
---------------------------------------------

.. Connect to a mothership (uplink)

Update your IncludeOS instance
------------------------------
