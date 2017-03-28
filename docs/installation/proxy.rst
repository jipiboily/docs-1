Proxy configuration
===================

Gemnasium Enterprise is designed to run behind your firewalls, inside your network.
If your network is using a proxy to access http or https resources, you may need to adapt GEE configuration.

By default, docker will use a `default network bridge <https://docs.docker.com/engine/userguide/networking/>`_, so the network stack is shared with the host where docker is running.

The proxy configuration for Docker is beyond the scope of this documentation, please refer to Docker and your operating system documentation if needed.

Gemnasium Enterprise is using the posix environment vars ``http_proxy`` (/``HTTP_PROXY``) and ``no_proxy`` (/``NO_PROXY``) directly. Set these variables according to your network configuration.
All the services inside the container will be aware of these two variables.


NO_PROXY configuration
----------------------

As Gemnasium Enterprise is composed of several services running inside the container, the communication between the services might be affected by a proxy configuration on your network.

Theses hosts must be added to your current ``no_proxy`` var inside the container:

- Your Gemnasium Enterprise full URL
- ``api``
- ``gemnasium-api``
- ``gemnasium-auth``
- ``gemnasium-badges``
- ``gemnasium-billing``
- ``gemnasium-notifier``
- ``gemnasium-repo-syncer``
- ``nsqlookupd``
- ``nsqd``
- ``postgresql``

Theses hostnames are added to ``/etc/hosts`` when the container is starting, so it's important that a ``curl gemnasium-api:8081`` is working inside the container.

.. code::

    root@60bdf6bb1ab5:/# curl gemnasium-api:8081
    <a href="http://docs.gemnasium.apiary.io">Moved Permanently</a>.


Remember that you can also pass an environment file to your docker container with ``--env-file=[file name]``, so this file should contain a complete ``no_proxy`` definition.
