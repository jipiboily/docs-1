Upgrade Gemnasium Enterprise
============================

While the data about packages, versions, etc. is automatically updated, and
stored in your local database, Gemnasium Enterprise won't upgrade itself
automatically.

Using latest version
--------------------

To upgrade Gemnasium Enterprise to a new version:

1- Pull the new image:

.. code::

    docker pull gemnasium/enterprise:latest

2- Stop and remove the container:

.. code::

    docker rm -f gemnasium

3- Run the image again (see :ref:`run_docker_image`.). Gemnasium Enterprise will update your data automatically, if necessary.

.. code::

    docker run [...]


Using nightly version
----------------------

Every night, a new build of Gemnasium Enterprise will be generated, as a ``gemnasium/enterprise:nightly`` image.
This image is intended for debugging only, and should not be used for production.

Using a tagged version
----------------------

The available tags are listed here:
https://hub.docker.com/r/gemnasium/enterprise/tags/

It is recommended to always use the ``latest`` tag.
