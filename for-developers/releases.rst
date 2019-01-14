Creating a release
------------------

To create a release simply run :code:`make release -e TAG=<desired tag>`. The release files will be available in the directory :code:`./release/<desired tag>`. The files will then need to be moved to the desired release location.

The only difference between the files created by :code:`make release` and :code:`make mothership` is that the release creates a mac binary in addition to the linux binary.
