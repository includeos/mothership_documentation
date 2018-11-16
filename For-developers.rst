.. _For-developers:

For developers
==============

Installation
------------

- Install `Go 1.10 <https://golang.org/dl/>`__
- Install `Docker <https://docs.docker.com/install/>`__
- Install `dep <https://golang.github.io/dep/>`__ (:code:`brew install dep` on macOS)
- Install `htpasswd <https://httpd.apache.org/docs/2.4/programs/htpasswd.html>`__ if you want to run your Mothership with authentication

::

    $ cd $GOPATH
    $ mkdir -p src/github.com/includeos && cd src/github.com/includeos
    $ git clone git@github.com:includeos/mothership.git
    $ make mothership-local # or ``make mothership`` if you want to start your Mothership in a Docker container

The Mothership GUI client
-------------------------
::

    $ git clone git@github.com:includeos/mothership_client.git # f.ex. in your HOME directory

MacOS::

    $ brew install node
    $ brew install npm
    $ npm install -g webpack@2.6.1
    $ npm install
    $ ./copyfiles.sh

Ubuntu::

    $ sudo apt install npm
    $ sudo npm install -g n # webpack needs the node command as opposed to nodejs. The npm n tool should fix that.
    $ sudo n stable
    $ sudo npm install -g webpack@2.6.1
    $ sudo npm install
    $ ./copyfiles.sh

Makefile
--------

The easiest way to build and test Mothership is by using the Makefile. This builds mothership using docker. The available options are::

    make help
    usage: make [target]

    Tests:
    tests             Run all the tests
    unit-tests        Run only the unit tests in docker
    integration-tests Run only the integration tests in docker

    Tools:
    gofmt            Run the gofmt check on the codebase, exits with 0 if OK
    errcheck         Run the errcheck tool, outputs how many errors are not checked
    whitespace       Run a whitespace check of the repo

    Miscellaneous:
    help             Show this help.
    release          Create a full release, set desired tag with -e TAG=<Desired-tag>
    clean            Delete all release folders in release/v*
    run              run mothership in docker

    Building:
    build            Build mothership binary in docker (mothership-linux-amd64)
    client           Build client binary in docker
    mothership       Build mothership in a docker container. Creates docker container named mothership

Creating a release
++++++++++++++++++

To create a release simply run :code:`make release -e TAG=<desired tag>`. The release files will be available in the directory :code:`./release/<desired tag>`. The files will then need to be moved to the desired release location.

The only difference between the files created by :code:`make release` and :code:`make mothership` is that the release creates a mac binary in addition to the linux binary.

Tests
-----

.. note:: If you want to run the tests locally then you need to create a docker volume with the htpasswd and certificate config files. To create this volume run the command:

    .. code-block:: bash

        docker run --rm \
          -v $PWD/test/test_config_files:/source \
          -v mship_test_config_files:/dest \
          ubuntu:xenial cp -r /source/. /dest

Run the tests locally
+++++++++++++++++++++

(Remember to clean your Mothership environment first by running :code:`./mothership serve --clean`)

.. code-block:: bash

    $ go test ./... -tags=all -p=1


The -tags option can be blank, :code:`all` or :code:`integration`, based on which tests you want to run.

Run the tests with Docker
+++++++++++++++++++++++++

Run the tests using the Makefile:

.. code-block:: bash

    $ make tests             # Run all tests
    $ make unit-tests        # Run only unit tests
    $ make integration-tests # Run only integration tests

Building IncludeOS docker containers
------------------------------------

To build your own docker container that can be used with Mothership use the following command::

  docker build \
  --label org.label-schema.build-date="$(git log -1 --format=%cd --date=iso8601-strict)" \
  --label org.label-schema.vcs-ref="$(git rev-parse --short HEAD)" \
  --label org.label-schema.version="$(git describe --tags --dirty)" \
  --label org.label-schema.name="IncludeOS_builder" \
  --label org.label-schema.vendor="IncludeOS" \
  -t includeos/builder:"$(git describe --tags --dirty)" --target=build .

This will give you all the labels that are used by Motheship to identify a version of IncludeOS. The labels that are in use are based on `label-schema convention draft rc1 <http://label-schema.org/rc1/>`__ :

build-date
  The timestamp of the last commit made to the version you are building.

vcs-ref
  The commit sha of the last commit.

version
  The version tag that IncludeOS will report it is using over uplink. This corresponds to the git describe output. Be careful with a dirty git directory as the version tag will display this.

name
  This is the predefined name that Mothership uses to filter by when looking for suitable images locally.

vendor
  This gives the name of the organization that is providing this image.

The final thing that can be changed is the :code:`includeos/builder` which is the name given to the image. When uploading to docker hub this is the name that is used to identify a docker hub repository.
