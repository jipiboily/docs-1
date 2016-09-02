Upgrade Gemnasium Enterprise
============================

While the data about packages, versions, etc. are automatically updaded, and
stored in your local database, Gemnasium Entreprise won't upgrade itself
automatically.

Using latest version
--------------------

Every night, a new build of Gemnasium Enterprise source code will generate a ``gemnasium/enterprise:latest`` image.

To upgrade Gemnasium Entreprise to a new version:

1- Stop the container:

    docker stop gemnasium

2- Remove the container:

    docker rm gemnasium

3- Pull the new image:

    docker pull gemnasium/entreprise:latest

4- Run the image again (see :ref:`run_docker_image`.). Gemnasium Enterprise will update your data automatically, if necessary.

Using tagged version
--------------------

Living on the edge is probably not a good idea on the long term.
Gemnasium will provide new tagged version for each release.

The available tags are listed here:
https://hub.docker.com/r/gemnasium/enterprise/tags/
