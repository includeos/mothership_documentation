.. _Developer-info:

Developer info
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
    $ dep ensure
    $ go build mothership.go

The Mothership GUI client
-------------------------
::

    $ git clone git@github.com:includeos/mothership_client.git // f.ex. in your HOME directory

MacOS::

    $ brew install node
    $ brew install npm
    $ npm install -g webpack@2.6.1
    $ npm install
    $ ./copyfiles.sh

Ubuntu::

    $ sudo apt install npm
    $ sudo npm install -g n // webpack needs the node command as opposed to nodejs. The npm n tool should fix that.
    $ sudo n stable
    $ sudo npm install -g webpack@2.6.1
    $ sudo npm install
    $ ./copyfiles.sh

Tests
-----

**Note**: If the tests are run with authentication (as they are by default), you need to add a user to the
mothership/config_files/.htpasswd file with the username `test` and the password `testtesttest` for the tests to pass.

To append the test user to an existing .htpasswd file::

    $ cd $GOPATH/src/github.com/includeos/mothership
    $ htpasswd -B config_files/.htpasswd test
    $ <enter password `testtesttest` when prompted to set a password for the user>

To create the .htpasswd file and add a user with the username `test` to it::

    $ htpasswd -c -B config_files/.htpasswd test
    $ <enter password `testtesttest` when prompted to set a password for the user>

Run the tests locally
~~~~~~~~~~~~~~~~~~~~~

(Remember to clean your Mothership environment first by running :code:`./mothership serve --clean`)

::

    $ go test ./... -tags=all -p=1

The -tags option can be blank, :code:`all` or :code:`integration`, based on which tests you want to run.

Run the tests with Docker
~~~~~~~~~~~~~~~~~~~~~~~~~

::

    $ ./build_mothership.sh -a

To get verbose output::

    $ ./build_mothership.sh -av
