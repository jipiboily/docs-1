Configure GitHub
================

Before being able to add projects from github.com, Gemnasium Enterprise needs to be configured to be able to access it.

There're parts to it. First, creating an OAuth application on GitHub and then configuring Gemnasium Enterprise to use that application.

Adding OAuth applications on GitHub
-----------------------------------

The first step we need to do create two new OAuth applications. You read that right; Gemnasium Enterprise needs two different OAuth applications. One to enable "Login with GitHub" and one to synchronize projects.

To do that:

- go on https://github.com/settings/developers
- click on the "Register a new application"
- Set the name to "Gemnasium Enterprise Login", Homepage URL to your Gemnasium Enterprise base URL (example: "https://gemnasium.example.com/") and finally the callback URL to be ```{homepage URL}/auth/auth/github.com/login/callback``` where you replace the home with your Gemnasium Enterprise host (example: `https://gemnasium.example.com/auth/auth/github.com/login/callback`)
- Click on the "Register application" button.

You will need some the client ID & secret you see on the confirmation page for the next section. You can keep that tab open and open a new tab to https://github.com/settings/developers to create the second application.

To create the second application required:

- go on https://github.com/settings/developers
- click on the "Register a new application"
- Set the name to "Gemnasium Enterprise Sync", Homepage URL to your Gemnasium Enterprise base URL (example: "https://gemnasium.example.com/") and finally the callback URL to be ```{homepage URL}/auth/auth/github.com/sync/callback``` where you replace the home with your Gemnasium Enterprise host (example: `https://gemnasium.example.com/auth/auth/github.com/sync/callback`)
- Click on the "Register application" button.


Configure Gemnasium Enterprise to use GitHub
--------------------------------------------

To be able to login and synchronize projects with GitHub, we need to first open a bash prompt into the Gemnasium Enterprise container.

.. code-block:: console

  docker exec -it gemnasium bash

Once this is done, customize and run the following commands in which you need to replace CLIENT_ID_FOR_LOGIN by the client ID you got from the "Gemnasium Enterprise Login" application we just created on GitHub and CLIENT_SECRET_FOR_LOGIN from the same GitHub application. Repeat for CLIENT_ID_FOR_SYNC & CLIENT_SECRET_FOR_SYNC but with the information from the "Gemnasium Enterprise Sync" application.

.. code-block:: console

  auth provider create github.com github https://github.com/login/oauth/authorize https://github.com/login/oauth/access_token
  auth clients create github.com login CLIENT_ID_FOR_LOGIN CLIENT_SECRET_FOR_LOGIN user:email
  auth clients create github.com sync CLIENT_ID_FOR_SYNC CLIENT_SECRET_FOR_SYNC write:repo_hook,read:org,repo
  repo-syncer sources create github.com GitHub github https://api.github.com


Once all those commands ran, you are ready to login with GitHub and add projects from it.
