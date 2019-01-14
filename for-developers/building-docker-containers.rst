Building IncludeOS Docker containers
------------------------------------

To build your own Docker container that can be used with Mothership use the following command::

  docker build \
  --label org.label-schema.build-date="$(git log -1 --format=%cd --date=iso8601-strict)" \
  --label org.label-schema.vcs-ref="$(git rev-parse --short HEAD)" \
  --label org.label-schema.version="$(git describe --tags --dirty)" \
  --label org.label-schema.name="IncludeOS_builder" \
  --label org.label-schema.vendor="IncludeOS" \
  -t includeos/builder:"$(git describe --tags --dirty)" --target=build .

This will give you all the labels that are used by Motheship to identify a version of IncludeOS. The labels that are in use are based on `label-schema convention draft rc1 <http://label-schema.org/rc1/>`__ :

build-date
  The timestamp of the last commit made to the version you are building.

vcs-ref
  The commit sha of the last commit.

version
  The version tag that IncludeOS will report it is using over uplink. This corresponds to the git describe output. Be careful with a dirty git directory as the version tag will display this.

name
  This is the predefined name that Mothership uses to filter by when looking for suitable images locally.

vendor
  This gives the name of the organization that is providing this image.

The final thing that can be changed is the :code:`includeos/builder` which is the name given to the image. When uploading to Docker hub this is the name that is used to identify a Docker hub repository.
