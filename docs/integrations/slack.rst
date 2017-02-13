Slack
=====

Because it's an enterprise version installed on your server, you need to do a few things in order to add Slack support.

The first step is to create a new Slack application, on Slack itself. Here are the steps:

- Go on https://api.slack.com/apps?new_app=1
- Set the App Name to be "Gemnasium Enterprise", and then click on the "Create App" button.
- You will need the Client ID and Client Secret in a moment. You can keep them around or go back to see them when you need them.
- (optional) By default, there is no Gemnasium icon for the messages sent on Slack. If you want it, which we suggest, you can upload it on the "Basic Information" screen of the app you just created, in the "Display Information" section. :download:`You can use this icon. <../_static/img/gemnasium-icon.png>`.
- Go in the "OAuth & Permissions" section (in the left menu) and set the redirect URL to https://gemnasium.example.com/auth/auth/slack.com/webhook/callback

Now, we need to configure Gemnasium Enterprise to use it:

.. code-block:: console

    docker exec -it gemnasium configure

Select "Slack", and then fill the corresponding fields with the values from the apps created above.

Now you only need to configure Slack notifications for a project in the web UI. You can do that in the "Settings" of each project.
