.. _Quick-start:

Quick start
===========

Dependencies
---------------
- `Docker <https://docs.docker.com/install/>`__
- `htpasswd <https://httpd.apache.org/docs/2.4/programs/htpasswd.html>`__

1  Create user
--------------
Mothership comes with basic auth authentication enabled by default. It is therefore necessary to create a user::

    $ touch .htpasswd               # Create empty .htpasswd file
    $ htpasswd -B .htpasswd myuser  # Add the user myuser

2  Configure TLS
----------------
TLS is also enabled by default. Mothership expects two files to be present in your directory:

:cert.pem: File containing your certificate.
:key.pem: File containing your private key

For instructions on how to generate a self signed certificate for testing see: :ref:`self-signed-tls`

3  Build and launch Mothership
------------------------------
First build then then run mothership using docker::

    $ docker build -t mothership .
    $ docker run \
        --name mothership \
        -p 8080:8080 \
        -p 9090:9090 \
        -v /var/run/docker.sock:/var/run/docker.sock \
        -v $PWD:/home/ubuntu/mothership/ \
        mothership serve

The commands before ``mothership`` are **docker** options::

    --name mothership                   Give docker container name
    -p 8080:8080                        Forward port 8080
    -p 9090:9090                        Forward port 9090
    -v /var/run/docker.sock:/var/run/docker.sock  Mount hosts docker process into container
    -v $PWD:/home/ubuntu/mothership/    Mount local dir into docker container

The commands after ``mothership`` are **mothership** options::

    mothership serve                    Start mothership server
