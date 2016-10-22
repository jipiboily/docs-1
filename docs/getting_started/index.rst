Getting Started
===============

What is Gemnasium Enterprise?
-----------------------------

Gemnasium Enterprise Edition (GEE) is the self-hosted version of Gemnasium, designed to run on your own servers, inside your network.
A private license key is required to use the software, otherwise no data will be synced with gemnasium.com (making your instance unusable).

To get a valid license key, please contact our `support <email:support@gemnasium.com>`_.

First steps
-----------

Gemnasium Enterprise is designed as a collaborative tool.
Each project must be added inside a team context, that's why the first thing to do is to create your team.
The name must be unique in the Gemnasium Enterprise instance.

Add your collegues
^^^^^^^^^^^^^^^^^^

Go to the "Team" section of the main menu, and press the "+ Add member" button to invite co-workers.
The role of the invited user will grant him/her access to your team:

- "Admin" users will be able to manage project and users
- "Basic" users will be able to access projects only

Add projects
^^^^^^^^^^^^

The first shortcut in the main menu "+ Add project" will allow you to add new projects to a team.
Once the team is selected, a source must be chosen. By default, only "offline" projects are available, because your instance doesn't know anything about external sources.

Offline projects will allow to upload dependencies files (Gemfile, Gemfile.lock, package.json,...) directly to a project. This kind of project is called "offline", because Gemnasium can't synchronize the files with an external source.

To add projects from github.com, or GitHub Enterprise, an external source must be created. Please refer to the corresponding pages in the "INSTALLATION AND CONFIGURATION" section of this documentation.
More source types will be available in the next versions of Gemnasium Enterprise.
