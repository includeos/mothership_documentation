.. _Quick-start:

Quick start
===========

Dependencies
------------
- Docker
- htpasswd

Create user
-----------
Mothership comes with basic auth authentication enabled by default. It is therefore necessary to create a user::

    $ htpasswd -c -B .htpasswd myuser // create a .htpasswd file and add the user myuser

TLS
---
TLS is also enabled by default. Mothership expects two files to be present in your directory:

:cert.pem: File containing your certificate.
:key.pem: File containing your private key

For instructions on how to generate a local certificate for testing see: :ref:`setup-tls`


Build and launch Mothership
---------------------------
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
