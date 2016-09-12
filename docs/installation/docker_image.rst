Run Gemnasium Enterprise
========================

Gemnasium Enterprise is shipped as a single, all-included `docker image <https://docs.docker.com/engine/tutorials/dockerimages/>`_.

Docker Hub Account
------------------

In order to be able to download the Gemnasium Enterprise docker image from the `Docker Hub <https://hub.docker.com/>`_, you must be
authenticated on docker hub, and provide the username used to pull the image to
Gemnasium.

.. todo: We'll probably leave the gemnasium/enterprise image public, to simplify the installation

.. _run_docker_image:

Run Docker Image
----------------

A persistent volume is needed to store Gemnasium Enterprise.

.. seealso:: Please refer to Docker Volumes for more information: https://docs.docker.com/engine/tutorials/dockervolumes/

Create a volume if needed::

    docker volume create --name gemnasium-data
    docker volume create --name gemnasium-logs

Run the image::

  docker run --detach  \
    --hostname gemnasium.example.com \
    --name gemnasium \
    --restart always \
    -e REDIRECT_HTTP_TO_HTTPS=false \
    -p 8080:80 \
    -v gemnasium-data:/var/opt/gemnasium/ \
    -v gemnasium-logs:/var/log/ \
    -v /var/run/docker.sock:/var/run/docker.sock \
    gemnasium/enterprise:latest

.. note:: Gemnasium needs the docker socket to be mount, since some internal jobs are launched inside containers.

This will pull and start Gemnasium Enterprise.
The service is available after a few seconds on http://gemnasium.example.org
(or any other domain you have passed as ``hostname``).

.. _ssl_configuration

SSL
^^^

The image is configured to serve with a default self-signed certificate.
To enable SSL support, the certificate file **must** be named after the server name.

Example: for `gemnasium.example.com`, the certificate files must be named:

* ``gemnasium.example.com.cert.pem`` for the certificate
* ``gemnasium.example.com.key.pem`` for its private key

If the certificate has an intermediate chain, it must concatenated after the server certificate::

    cat server.cert.pem ca-chain.cert.pem > gemnasium.example.com.cert.pem

The 2 files **must** be available in `/etc/gemnasium/ssl`:

.. code-block:: console
  :emphasize-lines: 5-6

  docker run --detach  \
    --hostname gemnasium.example.com \
    --name gemnasium \
    --restart always \
    -v /host/path/to/ssl/:/etc/gemnasium/ssl \
    -p 80:80 -p 443:443 \
    -v gemnasium-data:/var/opt/gemnasium/ \
    -v gemnasium-logs:/var/log/ \
    -v /var/run/docker.sock:/var/run/docker.sock \
    gemnasium/enterprise:latest

.. note:: The environment variable ``REDIRECT_HTTP_TO_HTTPS`` is `true` by default, and should be omited once ssl is configured.

If you need to use a different port for https than 443, use the ``EXTERNAL_URL`` env var to specify the full URL of your Gemnasium Enterprise server, including the port used:

.. code-block:: console
  :emphasize-lines: 7

  docker run --detach  \
    --hostname gemnasium.example.com \
    --name gemnasium \
    --restart always \
    -v /host/path/to/ssl/:/etc/gemnasium/ssl \
    -p 80:80 -p 8443:443 \
    -e EXTERNAL_URL=https://gemnasium.example.com:8443/ \
    -v gemnasium-data:/var/opt/gemnasium/ \
    -v gemnasium-logs:/var/log/ \
    -v /var/run/docker.sock:/var/run/docker.sock \
    gemnasium/enterprise:latest

SELinux
^^^^^^^
.. todo: test with SELinux in real conditions

Gemnasium Enterprise can't be run directly on SELinux servers, because:

1. the volumes will be readonly by default
2. the docker socket will be restricted to the host

Use this command instead:

.. code-block:: console
  :emphasize-lines: 6-8

  docker run --detach  \
    --hostname gemnasium.example.com \
    --name gemnasium \
    --restart always \
    -p 8080:80 \
    -v gemnasium-data:/var/opt/gemnasium/:Z \
    -v gemnasium-logs:/var/log/:Z \
    -v /var/run/docker.sock:/var/run/docker.sock:Z \
    gemnasium/enterprise:latest

This will label the content inside the container with the exact MCS label that
the container will run with, basically it runs ``chcon -Rt svirt_sandbox_file_t
-l s0:c1,c2 /var/db`` where ``s0:c1,c2`` differs for each container.

.. seealso:: More info: http://www.projectatomic.io/blog/2015/06/using-volumes-with-docker-can-cause-problems-with-selinux/

Please refer to this project to install the proper SELinux module to fix the second point.

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

Finally, as explained in the :ref:`_ssl_configuration` section, your certificate and key must be available in the `/etc/gemnasium/ssl` folder.


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
    --hostname gemnasium.example.com \
    --name gemnasium \
    --restart always \
    -p 8080:80 \
    -v gemnasium-data:/var/opt/gemnasium/ \
    -v gemnasium-logs:/var/log/ \
    -v /var/run/docker.sock:/var/run/docker.sock \
    -e GRAYLOG_SERVICE_HOST=logs.example.log
    -e GRAYLOG_SERVICE_PORT=1515
    gemnasium/enterprise:latest

Both variables must be set to activate the GELF output.

.. note: Logs will still be available in the container logs (stdout)

Obtaining a shell
----------------

The docker image doesn't a SSH server, because docker provides everything needed to get a shell console inside the container::

    docker exec -it gemnasium bash

will create a new bash session, with the root user.

.. warning:: With great power comes great responsability: as root, you can damage files inside the container, including persisted data.

