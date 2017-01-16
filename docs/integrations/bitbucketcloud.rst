Bitbucket Cloud
===============

Before being able to add projects from bitbucket.org (A.K.A. Bitbucket Cloud), Gemnasium Enterprise needs to be configured to be able to access it.

First add an OAuth consumer on bitbucket.org, then configure Gemnasium Enterprise to use that OAuth consumer.

.. _bitbucket_cloud_add_consumer:

Adding OAuth consumers on Bitbucket.org
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The first step is to go to open the "Add OAuth consumer" form:

- Go to the Bitbucket Settings page.
- If needed, use the dropdown list and select the Bitbucket account of your company.
- Click on the "OAuth" item in the sidebar.
- Click on the "Add consumer" button.

Or simply visit: https://bitbucket.org/account/user/[your_account]/oauth-consumers/new

Then fill the form:

- Set the name to "Gemnasium Enterprise".
- Set the callback URL to be ``{homepage URL}/auth/auth/bitbucket.org`` where you replace the home with your Gemnasium Enterprise host (example: `https://gemnasium.example.com/auth/auth/bitbucket.org`)
- Set the URL to your Gemnasium Enterprise base URL (example: "https://gemnasium.example.com/").
- Grant permissions to: read account, read repositories, read and write webhooks.
- Save the new OAuth consumer.

You'll then be redirected to the OAuth Consumers page and the consumer named "Gemnasium Enterprise" will be visible in the list. Click on its name to expand the item and retrieve the credentials: the consumer key and its secret.

Configure Gemnasium Enterprise to use Bitbucket.org
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A convenient script is provided to configure your instance:

.. code-block:: console

  docker exec -it gemnasium configure

Your Gemnasium Enterprise users are now able to login using their Bitbucket.org account.
A new source named "Bitbucket.org" is also available on the "Add Project" screen.

Advanced configuration
^^^^^^^^^^^^^^^^^^^^^^

The default configuration described above enables both Bitbucket.org Sign In and project synchronization with Bitbucket.org. The advanced configuration makes it possible to restrict the integration to one of these two features.

To enable Bitbucket.org Sign Up, add an OAuth consumer with the permissions "account" and "email", then run these commands to configure Gemnasium Enterprise accordingly:

.. code-block:: console

  docker exec -it gemnasium auth provider create bitbucket.org bitbucket https://bitbucket.org/site/oauth2/authorize https://bitbucket.org/site/oauth2/access_token
  docker exec -it gemnasium auth clients create bitbucket.org login OAUTH_CONSUMER_KEY OAUTH_CONSUMER_SECRET account,email

To enable Bitbucket.org Synchronization, add an OAuth consumer with the permissions "repository" and "webhook", then run these commands to configure Gemnasium Enterprise accordingly:

.. code-block:: console

  docker exec -it gemnasium auth provider create bitbucket.org bitbucket https://bitbucket.org/site/oauth2/authorize https://bitbucket.org/site/oauth2/access_token
  docker exec -it gemnasium auth clients create bitbucket.org sync OAUTH_CONSUMER_KEY OAUTH_CONSUMER_SECRET repository,webhook
  docker exec -it gemnasium repo-syncer sources create bitbucket.org "Bitbucket Cloud" bitbucket https://api.bitbucket.org

``OAUTH_CONSUMER_KEY`` and ``OAUTH_CONSUMER_SECRET`` must replaced with the key and the secret of the OAuth consumer created on Bitbucket.org.
See :ref:`bitbucket_cloud_add_consumer` above.
