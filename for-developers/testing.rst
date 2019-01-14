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
