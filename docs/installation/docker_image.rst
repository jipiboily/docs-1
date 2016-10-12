Run Gemnasium Enterprise
========================

Gemnasium Enterprise is shipped as a single, all-included `docker image <https://docs.docker.com/engine/tutorials/dockerimages/>`_.

Docker Hub Account
------------------

In order to be able to download the Gemnasium Enterprise docker image from the `Docker Hub <https://hub.docker.com/>`_, you must be
authenticated on docker hub, and provide the username used to pull the image to
Gemnasium.

To have access to the Gemnasium Enterprise image, send us the Docker Hub user you want; we will share the image with that user.

.. _run_docker_image:

Preparing volumes
-----------------

Persistent volumes are needed to store Gemnasium Enterprise data.
The easiest way to get started, is to create local volumes on your server, but it can be any kind of volume supported by the docker engine.

.. seealso:: Please refer to Docker Volumes for more information: https://docs.docker.com/engine/tutorials/dockervolumes/

To create local volumes, on you server::

    docker volume create --name gemnasium-data
    docker volume create --name gemnasium-logs

.. _ssl_configuration:

Configuring SSL
---------------

A valid certificate must be provided to run Gemnasium Enterprise with the integrated SSL server (nginx).
If you don't need Gemnasium Enterprise to serve content on https directly, go directly to the section: :ref:`run_without_ssl`.

The certificate files **must** be named after the server name.

Example: for `gemnasium.example.com`, the certificate files **must** be named:

* ``gemnasium.example.com.cert.pem`` for the certificate
* ``gemnasium.example.com.key.pem`` for its private key

Gemnasium will look for 2 files with the ``.cert.pem`` and ``.key.pem`` suffix.

If the certificate has an intermediate chain, it must concatenated after the server certificate::

    cat server.cert.pem ca-chain.cert.pem > gemnasium.example.com.cert.pem

The 2 files **must** be available in ``/etc/gemnasium/ssl``, inside the container.

.. code-block:: console

  docker run --detach  \
    --name gemnasium \
    --restart always \
    -v /host/path/to/ssl/:/etc/gemnasium/ssl \
    -p 80:80 -p 443:443 \
    -v gemnasium-data:/var/opt/gemnasium/ \
    -v gemnasium-logs:/var/log/ \
    -v /var/run/docker.sock:/var/run/docker.sock \
    gemnasium/enterprise:latest

.. note:: Gemnasium needs the docker socket to be mount, since some internal jobs are launched inside containers.

This will pull and start Gemnasium Enterprise. Your instance will be available at https://gemnasium.example.com after a few seconds.

If you need to use a different port for https than 443, use the ``EXTERNAL_URL`` env var to specify the full URL of your Gemnasium Enterprise server, including the port used:

.. code-block:: console
  :emphasize-lines: 6

  docker run --detach  \
    --name gemnasium \
    --restart always \
    -v /host/path/to/ssl/:/etc/gemnasium/ssl \
    -p 80:80 -p 8443:443 \
    -e EXTERNAL_URL=https://gemnasium.example.com:8443/ \
    -v gemnasium-data:/var/opt/gemnasium/ \
    -v gemnasium-logs:/var/log/ \
    -v /var/run/docker.sock:/var/run/docker.sock \
    gemnasium/enterprise:latest

and start browsing https://gemnasium.example.com:8443/

.. _run_without_ssl:

Running without SSL
-------------------

.. warning:: We strongly discourage running Gemnasium Enterprise without any SSL termination. This section is present if you already have SSL terminations, like secured reverse-proxies, ssl appliances, etc.

Run the image::

  docker run --detach  \
    --name gemnasium \
    --restart always \
    -e REDIRECT_HTTP_TO_HTTPS=false \
    -p 80:80 \
    -v gemnasium-data:/var/opt/gemnasium/ \
    -v gemnasium-logs:/var/log/ \
    -v /var/run/docker.sock:/var/run/docker.sock \
    gemnasium/enterprise:latest

.. note:: The environment variable ``REDIRECT_HTTP_TO_HTTPS`` is `true` by default, and must be ``false`` in this case.

The service is available after a few seconds on the port 80 of your server.


SELinux
^^^^^^^
.. todo: test with SELinux in real conditions

Gemnasium Enterprise can't be run directly on SELinux servers, because:

1. The volumes will be readonly by default
2. The docker socket will be restricted to the host

Use this command instead:

.. code-block:: console
  :emphasize-lines: 6-8

  docker run --detach  \
    --name gemnasium \
    --restart always \
    -v /host/path/to/ssl/:/etc/gemnasium/ssl \
    -p 80:80 -p 443:443 \
    -v gemnasium-data:/var/opt/gemnasium/:Z \
    -v gemnasium-logs:/var/log/:Z \
    -v /var/run/docker.sock:/var/run/docker.sock:Z \
    gemnasium/enterprise:latest

This will label the content inside the container with the exact MCS label that
the container will run with, basically it runs ``chcon -Rt svirt_sandbox_file_t
-l s0:c1,c2 /var/db`` where ``s0:c1,c2`` differs for each container.

.. seealso:: More info: http://www.projectatomic.io/blog/2015/06/using-volumes-with-docker-can-cause-problems-with-selinux/

Please refer to this project to install the proper SELinux module to fix the second point.

.. _docker_image_volumes:

Volumes
^^^^^^^

Gemnasium is storing data in two folders, which should be mounted as volumes

========================  ========================  ================
Local location            Location in container     Usage
========================  ========================  ================
gemnasium-data (volume)   /var/opt/gemnasium        Gemnasium data
gemnasium-logs (volume)   /var/log                  Gemnasium logs
========================  ========================  ================

Gemnasium data is composed mostly of the PostgreSQL database files, but also nsq data, etc.
These files must be backed up, refer to the :doc:`backup`. section.

The ``/var/log`` contains the OS logs, and everything dedicated to gemnasium in ``/var/log/gemnasium``.

.. note: The logs files are rotated automatically.

Finally, as explained in the :ref:`ssl_configuration` section, your certificate and key must be available in the ``/etc/gemnasium/ssl`` folder.


Logging
-------

By default, all logs will be sent to the standard output of the container
(``stdout``), along with files in ``/var/log``. This makes it easier to troubleshoot if needed.

Graylog
^^^^^^^

Gemnasium Enterprise can be configured to log to a distant `Graylog <https://www.graylog.org>`_ server.
To enable this feature, use the following environment variables:

====================  ===========================
Env variables         Usage
====================  ===========================
GRAYLOG_SERVICE_HOST  Graylog input hostname/ip
GRAYLOG_SERVICE_PORT  Graylog input port
====================  ===========================

Example:

.. code-block:: console
  :emphasize-lines: 9,10

  docker run --detach  \
    --name gemnasium \
    --restart always \
    -v /host/path/to/ssl/:/etc/gemnasium/ssl \
    -p 80:80 -p 443:443 \
    -v gemnasium-data:/var/opt/gemnasium/ \
    -v gemnasium-logs:/var/log/ \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -e GRAYLOG_SERVICE_HOST=logs.example.log
    -e GRAYLOG_SERVICE_PORT=1515
    gemnasium/enterprise:latest

Both variables must be set to activate the GELF output.

.. note: Logs will still be available in the container logs (stdout)

Obtaining a shell
-----------------

The docker image doesn't have a SSH server, because docker provides everything needed to get a shell console inside the container::

    docker exec -it gemnasium bash

will create a new bash session, with the root user.

.. warning:: With great power comes great responsability: as root, you can damage files inside the container, including your persisted data.

