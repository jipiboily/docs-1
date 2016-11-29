GitHub.com
==========

Before being able to add projects from github.com, Gemnasium Enterprise needs to be configured to be able to access it.

There're parts to it. First, creating an OAuth application on GitHub and then configuring Gemnasium Enterprise to use that application.

Adding OAuth applications on GitHub
-----------------------------------

The first step we need to do create two new OAuth applications. You read that right; Gemnasium Enterprise needs two different OAuth applications. One to enable "Login with GitHub" and one to synchronize projects.

To do that:

- go on https://github.com/settings/developers
- click on the "Register a new application"
- Set the name to "Gemnasium Enterprise Login", Homepage URL to your Gemnasium Enterprise base URL (example: "https://gemnasium.example.com/") and finally the callback URL to be ``{homepage URL}/auth/auth/github.com/login/callback`` where you replace the home with your Gemnasium Enterprise host (example: `https://gemnasium.example.com/auth/auth/github.com/login/callback`)
- Click on the "Register application" button.

You will need some the client ID & secret you see on the confirmation page for the next section. You can keep that tab open and open a new tab to https://github.com/settings/developers to create the second application.

To create the second application required:

- go on https://github.com/settings/developers
- click on the "Register a new application"
- Set the name to "Gemnasium Enterprise Sync", Homepage URL to your Gemnasium Enterprise base URL (example: "https://gemnasium.example.com/") and finally the callback URL to be ``{homepage URL}/auth/auth/github.com/sync/callback`` where you replace the home with your Gemnasium Enterprise host (example: `https://gemnasium.example.com/auth/auth/github.com/sync/callback`)
- Click on the "Register application" button.


Configure Gemnasium Enterprise to use GitHub
--------------------------------------------

A convenient script is provided to add everything at once. 

.. code-block:: console

  docker exec -it gemnasium configure

Your Gemnasium Enterprise users are now able to login using their GitHub account.
A new source named "GitHub" is also available on the "Add Project" screen.
