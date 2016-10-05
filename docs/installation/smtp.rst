SMTP setup
==========

In order to send notifications (team invitations, digests, etc.), Gemnasium Enterprise needs a SMTP server.
There is no such server included in the docker image, so an external SMTP server should be used.
Note that SMTP connectivity is not fully required, just recommended.


Configuration
-------------

SMTP can be configured by passing environment variable to the docker container:

====================  ===========================
Env variables         Usage
====================  ===========================
SMTP_SERVICE_HOST     Server host address
SMTP_SERVICE_PORT     Server port
SMTP_USER_NAME        Username
SMTP_PASSWORD         Password
SMTP_INSECURE         Skip SSL verification
====================  ===========================

The SMTP password can be plain auth, or a secret (CRAM-MD5 auth).
These variables are all optional.

SSL and TLS are supported, and will be automatically used if the port (``SMTP_SERVICE_PORT``) is ``465`` or ``587``.
