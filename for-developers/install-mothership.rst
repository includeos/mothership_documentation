
Installing Mothership
---------------------

- Install `Go 1.11 <https://golang.org/dl/>`__
- Install `bzr <https://bazaar.canonical.com/en/>`__ as it is used by one of our dependencies. (brew install bzr works)
- Install `htpasswd <https://httpd.apache.org/docs/2.4/programs/htpasswd.html>`__ if you want to run your Mothership with authentication

With Go 1.11 there is no longer a need to use ``GOPATH``. Dependencies are now handled by go modules.
::

    $ git clone git@github.com:includeos/mothership.git
    $ go build
