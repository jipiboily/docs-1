Environment Variables
=====================

Gemnasium Enterprise is entirely configured using environment variables.

Variables persistence
^^^^^^^^^^^^^^^^^^^^^

If you don't want to specify by hand all environment variables in docker run, you can create a file including all the variables to be set:
Compose expects each line in an env file to be in VAR=VAL format. Lines beginning with # (i.e. comments) are ignored, as are blank lines.

To use the environment file, add the ``--env-file=[file name]`` to your ``docker run`` command line.

With docker-compose, the name should be named ``.env`` in the same directory as your ``docker-compose.yaml`` file, and will be loaded automatically. 
Remember to specify the env variables expected in the file, otherwise docker-compose will simply ignore them.

Variables List
^^^^^^^^^^^^^^

+---------------------------+----------------------------------+------------------------------------+-------------------+
| Env variables             | Usage                            | Required                           | Default           |
+===========================+==================================+====================================+===================+
| Main                                                                                                                  |
+---------------------------+----------------------------------+------------------------------------+-------------------+
| ``LICENSE_KEY``           | Your GEE private license key     | required                           |                   |
+---------------------------+----------------------------------+------------------------------------+-------------------+
| ``EXTERNAL_URL``          | Your GEE full URL                | required                           |                   |
+---------------------------+----------------------------------+------------------------------------+-------------------+
| Logging                                                                                                               |
+---------------------------+----------------------------------+------------------------------------+-------------------+
| ``GRAYLOG_SERVICE_HOST``  | Graylog input hostname/ip        | optional                           |                   |
+---------------------------+----------------------------------+------------------------------------+-------------------+
| ``GRAYLOG_SERVICE_PORT``  | Graylog input port               | if ``GRAYLOG_SERVICE_HOST`` is set |                   |
+---------------------------+----------------------------------+------------------------------------+-------------------+
| SMTP                                                                                                                  |
+---------------------------+----------------------------------+------------------------------------+-------------------+
| ``SMTP_SERVICE_HOST``     | SMTP server host                 | optional                           |                   |
+---------------------------+----------------------------------+------------------------------------+-------------------+
| ``SMTP_SERVICE_PORT``     | SMTP server port                 | optional                           | 25                |
+---------------------------+----------------------------------+------------------------------------+-------------------+
| ``SMTP_USER_NAME``        | SMTP user                        | optional                           |                   |
+---------------------------+----------------------------------+------------------------------------+-------------------+
| ``SMTP_PASSWORD``         | SMTP password (plain auth) or    | optional                           |                   |
|                           | secret (CRAM-MD5 auth)           |                                    |                   |
+---------------------------+----------------------------------+------------------------------------+-------------------+
| ``SMTP_INSECURE``         | SMTP skip SSL verification       | optional                           | false             |
+---------------------------+----------------------------------+------------------------------------+-------------------+
| ``MAILER_EMAIL_FROM``     | Email notifications sender       | optional                           | app@gemnasium.com |
+---------------------------+----------------------------------+------------------------------------+-------------------+
