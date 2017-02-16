Self-Signed Certificates
========================

Gemnasium Enterprise doesn't require a certificate signed by a (well-)known authority.
As with any other service running on your network, it's not recommanded to use such certificate, unless a CA certificate is being used to sign it.

Gemnaasium Enterprise will fail to contact your local servers if they are using such certificate, unless the right CA is available in the container.

.. note:: Only valid (ie: signed by a known authority) certificates will work with Gemnasium Enterprise.

Installing a CA CERT
--------------------

Gemnasium Enterprise is using a Debian Stable as the operating system. To make sure all services inside the container are using the right certificates, they must be installed into ``/etc/ssl/certs/``.
Do not mount a folder on this path directly, as you may hide the current certificates already present (Debian default ones).
Each certificate can actually be mounted:

.. code::

    docker run [...] -v [ca-cert file path]:/etc/ssl/certs/[ca-cert file path]:ro gemnasium/enterprise


.. note:: If you have more than one CA certificate, replicate this parameter for each.

Getting a valid certificate
---------------------------

If you don't have a valid authority to sign your certificates, you may want to use `Let's Encrypt <https://letsencrypt.org/>`_.
They are delivering free certificates.
