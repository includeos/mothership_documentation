.. _Tests:

Tests
=====

**Note**: If the tests are run with authentication (as they are by default), you need to add a user to the mothership/.htpasswd file with the username `test` and the password `testtesttest` for the tests to pass.
To create the authentication file .htpasswd and add a user with the username `test` to it:

::

    $ cd $GOPATH/src/github.com/includeos/mothership
    $ htpasswd -c -B .htpasswd test
    $ <enter password `testtesttest` when prompted to set a password for the user>

Run the tests locally
---------------------

(Remember to clean your Mothership environment first by running :code:`./mothership serve --clean`)

::

    $ go test ./... -tags=all -p=1

The -tags option can be blank, :code:`all` or :code:`integration`, based on which tests you want to run.

Run the tests with Docker
-------------------------

::

    $ ./build_mothership.sh -a

To get verbose output:

::

    $ ./build_mothership.sh -av
