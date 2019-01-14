.. _Go modules:

Go Modules
----------

With Go 1.11 Go modules is the new official way of handling dependencies. It is available as an experimental feature in Go 1.11 and will enabled by default starting with Go 1.12.

One of the main visible benefits is being able to keep the source files outside the ``$GOPATH``. If your Mothership repo is outside the GOPATH then go modules will be enabled by default.

Go modules is implemented into all existing Go tools, so all commands you are used to using will still work, only now with go modules being used on the backend.

Check if all dependencies are correct
+++++++++++++++++++++++++++++++++++++

The basic command to make sure you are in the correct state is::

  go mod verify

Adding a new dependency
+++++++++++++++++++++++

To add a new dependency we use same old syntax as we are used to with Go::

  go get -u <dependency name>


Updating dependencies
+++++++++++++++++++++

Taken from `here <https://github.com/golang/go/wiki/Modules#how-to-upgrade-and-downgrade-dependencies>`__.

To view available minor and patch upgrades for all direct and indirect dependencies, run::

   go list -u -m all

To upgrade to the latest version for all direct and indirect dependencies of the current module::

    go get -u        # to use the latest minor or patch releases
    go get -u=patch  # to use the latest patch releases
