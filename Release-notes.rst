.. _Release notes:

Release notes
=============

v0.12 March 9, 2018
-------------------

Internal improvements
~~~~~~~~~~~~~~~~~~~~~

- Authentication, TLS and docker builder are default when starting Mothership
- Improved logging

GUI
~~~

- Search functionality on the Instances, Images and NaCl pages
    - Instance search targets:
        - ID
        - UUID
        - Alias
        - Description
        - IP addresses
        - Running image's checksum (ID)
        - Devices
    - Image search targets:
        - Checksum (ID)
        - Name
        - OS version
        - NaCl name
    - NaCl search targets:
        - ID
        - Name
        - Content
- Pagination on the Instances, Images and NaCl pages (20 elements per page)
- Description field added per instance, which is persistent
