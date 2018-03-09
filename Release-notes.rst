.. _Release notes:

Release notes
=============

v0.12 March 2018
----------------

Internal improvements
~~~~~~~~~~~~~~~~~~~~~

- Authentication, TLS and docker builder are default when starting Mothership
- Improved logging

GUI
~~~

- Description field added per instance, which is persistent

.. image:: _static/images/release-notes-v0.12/instance-description.png

- Search functionality on the Instances, Images and NaCl pages

    - Image search targets:
        - Checksum (ID)
        - Name
        - OS version
        - NaCl name
    - Instance search targets:
        - ID
        - UUID
        - Alias
        - Description
        - IP addresses
        - Running image's checksum (ID)
        - Devices
    - NaCl search targets:
        - ID
        - Name
        - Content

.. image:: _static/images/release-notes-v0.12/search-images.png

.. image:: _static/images/release-notes-v0.12/search-instances.png

.. image:: _static/images/release-notes-v0.12/search-nacl.png

- Pagination on the Instances, Images and NaCl pages (20 elements per page)

.. image:: _static/images/release-notes-v0.12/pagination.png
