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
