GitHub
======

GitHub.com
----------

Before being able to add projects from github.com, Gemnasium Enterprise needs to be configured to be able to access it.

There're parts to it. First, creating an OAuth application on GitHub and then configuring Gemnasium Enterprise to use that application.

Adding OAuth applications on GitHub
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The first step we need to do create two new OAuth applications. You read that right; Gemnasium Enterprise needs two different OAuth applications. One to enable "Login with GitHub" and one to synchronize projects.

To do that:

- go on https://github.com/settings/developers (or alternatively, add the application to an organization: https://github.com/organizations/[your_org]/settings/applications)
- click on the "Register a new application"
- Set the name to "Gemnasium Enterprise Login", Homepage URL to your Gemnasium Enterprise base URL (example: "https://gemnasium.example.com/") and finally the callback URL to be ``{homepage URL}/auth/auth/github.com/login/callback`` where you replace the home with your Gemnasium Enterprise host (example: `https://gemnasium.example.com/auth/auth/github.com/login/callback`)
- Click on the "Register application" button.

You will need some the "Client ID" and "Client Secret" you see on the confirmation page for the next section. You can keep that tab open and open a new tab to https://github.com/settings/developers to create the second application.

To create the second application required:

- go on https://github.com/settings/developers
- click on the "Register a new application"
- Set the name to "Gemnasium Enterprise Sync", Homepage URL to your Gemnasium Enterprise base URL (example: "https://gemnasium.example.com/") and finally the callback URL to be ``{homepage URL}/auth/auth/github.com/sync/callback`` where you replace the home with your Gemnasium Enterprise host (example: `https://gemnasium.example.com/auth/auth/github.com/sync/callback`)
- Click on the "Register application" button.


Configure Gemnasium Enterprise to use GitHub
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

A convenient script is provided to configure your instance:

.. code-block:: console

  docker exec -it gemnasium configure

Your Gemnasium Enterprise users are now able to login using their GitHub account.
A new source named "GitHub" is also available on the "Add Project" screen.


GitHub Enterprise
-----------------

GitHub Enterprise is no different than GitHub, the steps are the same as above, just replace `github.com` with your GitHub Enterprise instance URL.

When using the configuration script, make sure to select "GitHub Enterprise" instead of "GitHub.com". The script will ask for your instance URL, and to name it.

Several GitHub Enterprise instances can be configured, just name them accordingly.

Requirements
^^^^^^^^^^^^

Gemnasium Enterprise has been tested against GitHub Enterprise >=2.7.X.
If your version is older than 2.7 series, please contact our support.
