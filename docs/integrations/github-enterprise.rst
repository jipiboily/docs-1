GitHub Enterprise
=================

Before being able to add projects from your GitHub Enterprise (GHE) instance, Gemnasium Enterprise needs to be configured to be able to access it.

There're two parts to it. First, creating an OAuth application on GitHub Enterprise and then configuring Gemnasium Enterprise to use that application.

Adding an OAuth application on GitHub Enterprise
------------------------------------------------

The first step we need to do create a new OAuth application.

To do that:

- go on https://ghe.example.com/settings/developers where `ghe.example.com` is the hostname of your GHE instance
- click on the "Register a new application"
- Set the name to "Gemnasium Enterprise Login", Homepage URL to your Gemnasium Enterprise base URL (example: "https://gemnasium.example.com/") and finally the callback URL to be ``{homepage URL}/auth/auth/{GitHub Enterprise host}/login/callback`` where you replace the home and the GHE host (example: `https://gemnasium.example.com/auth/auth/github-enterprise.example.com/login/callback`)
- Click on the "Register application" button.

You will need some the client ID & secret you see on the confirmation page for the next section. You can keep that tab open and open a new tab to https://github.com/settings/developers to create the second application.

To create the second application required:

- go on https://ghe.example.com/settings/developers where `ghe.example.com` is the hostname of your GHE instance
- click on the "Register a new application"
- Set the name to "Gemnasium Enterprise Sync", Homepage URL to your Gemnasium Enterprise base URL (example: "https://gemnasium.example.com/") and finally the callback URL to be ``{homepage URL}/auth/auth/{GitHub Enterprise host}/sync/callback`` where you replace the home and the GHE host (example: `https://gemnasium.example.com/auth/auth/github-enterprise.example.com/sync/callback`)
- Click on the "Register application" button.

Configure Gemnasium Enterprise to use GitHub Enterprise
-------------------------------------------------------

A convenient script is provided to add everything at once:

.. code-block:: console

  docker exec -it gemnasium configure

Select "GitHub Enterprise", and then fill the corresponding fields with the values from the apps created above.

Your Gemnasium Enterprise users are now able to login using their GitHub Enterprise account.
A new source with the name entered in the configure script is also available on the "Add Project" screen.
