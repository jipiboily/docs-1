Configure GitHub Enterprise
===========================

Before being able to add projects from your GitHub Enterprise (GHE) instance, Gemnasium Enterprise needs to be configured to be able to access it.

There're two parts to it. First, creating an OAuth application on GitHub Enterprise and then configuring Gemnasium Enterprise to use that application.

Adding an OAuth application on GitHub Enterprise
------------------------------------------------

The first step we need to do create a new OAuth application.

To do that:

- go on https://ghe.example.com/settings/developers where `ghe.example.com` is the hostname of your GHE instance
- click on the "Register a new application"
- Set the name to "Gemnasium Enterprise Login", Homepage URL to your Gemnasium Enterprise base URL (example: "https://gemnasium.example.com/") and finally the callback URL to be ```{homepage URL}/auth/auth/{GitHub Enterprise host}/login/callback``` where you replace the home and the GHE host (example: `https://gemnasium.example.com/auth/auth/github-enterprise.example.com/login/callback`)
- Click on the "Register application" button.

You will need some the client ID & secret you see on the confirmation page for the next section. You can keep that tab open and open a new tab to https://github.com/settings/developers to create the second application.

To create the second application required:

- go on https://ghe.example.com/settings/developers where `ghe.example.com` is the hostname of your GHE instance
- click on the "Register a new application"
- Set the name to "Gemnasium Enterprise Sync", Homepage URL to your Gemnasium Enterprise base URL (example: "https://gemnasium.example.com/") and finally the callback URL to be ```{homepage URL}/auth/auth/{GitHub Enterprise host}/sync/callback``` where you replace the home and the GHE host (example: `https://gemnasium.example.com/auth/auth/github-enterprise.example.com/sync/callback`)
- Click on the "Register application" button.

Configure Gemnasium Enterprise to use GitHub Enterprise
-------------------------------------------------------

To be able to login and synchronize projects with GitHub Enterprise, we need to first open a bash prompt into the Gemnasium Enterprise container.

.. code-block:: console

  docker exec -it gemnasium bash

Once this is done, customize and run the following commands in which you need to replace `github-enterprise.example.com` by your GHE instance host, CLIENT_ID_FOR_LOGIN by the client ID you got from the "Gemnasium Enterprise Login" application we just created on GitHub and CLIENT_SECRET_FOR_LOGIN from the same GitHub application. Repeat for CLIENT_ID_FOR_SYNC & CLIENT_SECRET_FOR_SYNC but with the information from the "Gemnasium Enterprise Sync" application.

.. code-block:: console

  auth provider create github-enterprise.example.com github https://github-enterprise.example.com/login/oauth/authorize https://github-enterprise.example.com/login/oauth/access_token
  auth clients create github-enterprise.example.com login CLIENT_ID_FOR_LOGIN CLIENT_SECRET_FOR_LOGIN user:email
  auth clients create github-enterprise.example.com sync CLIENT_ID_FOR_SYNC CLIENT_SECRET_FOR_SYNC write:repo_hook,read:org,repo
  repo-syncer sources create github-enterprise.example.com "GitHub-Enterprise" github https://github-enterprise.example.com/api/v3


Once all those commands ran, you are ready to login with GitHub Enterprise and add projects from it.
