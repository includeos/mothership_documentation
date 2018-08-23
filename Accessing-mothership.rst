.. _Accessing-mothership:

Accessing Mothership
====================

cURL
----

To perform authenticated requests with TLS using curl (which is default when starting Mothership), add the :code:`-u` flag to your curl commands:

::

    $ curl -u <username>:<password> https://localhost:8080/images -k

CLI
---

To enable the CLI commands to send your credentials when making requests, you can either add the :code:`--username` and :code:`--password` flags to your CLI commands,
or perform :code:`./mothership init` to have the client prompt you for your credentials and store them in a config file for future CLI commands.

::

    $ ./<mothership-binary> images --username <username> --password <password>

.. _the-website:

Website
-------

When you have come this far, or if you want to connect to a public Mothership that is already running, you can open
your browser and go to the Mothership's website.

If you are running a Mothership locally, you will find your Mothership's website by going to
`https://localhost:8080 <https://localhost:8080>`__ if you have started your Mothership with TLS enabled.

If you have started your Mothership without TLS, you will find the website at `http://localhost:8080 <http://localhost:8080>`__.

Here, if the Mothership was started with authentication enabled, you will see a Login-page:

.. image:: _static/images/login-button.png

When clicking on this, a popup will appear, asking you to fill in your username and password:

.. image:: _static/images/login.png

This username and password must match one of the entries in the previously created mothership/config_files/.htpasswd file.

If no authentication is required, you will be sent straight to the Instances page:

.. image:: _static/images/instances-start.png
