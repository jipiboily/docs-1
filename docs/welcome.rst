Welcome
=======

Welcome to the Gemnasium Enterprise |version| documentation, where you can find information and guides to help you with Gemnasium Enterprise and start exploring its features.

Use the left navigation bar to browse the documentation, the Search bar in the top-left to look for something specific, or the links below to access some highlights.

.. note:: Gemnasium Enterprise Edition will be referred as "GEE" in this documentation

Release Notes
=============

1.0.0-beta3 - 2016-11-29
------------------------

* [FEATURE] GitLab Support
* [FEATURE] New notifications in the UI


Known issues:

* [BUG][GITLAB] Symlinks on dependency files are not followed
* [BUG][GITLAB] Dependency files greater than 2MB are ignored
* [BUG] Can't sign-in using an oauth account if the same email is already used

1.0.0-beta2 - 2016-11-18
------------------------

* [FEATURE] Display commits in project page
* [FEATURE] Internal logging (live feeds will be available in beta3)

* [BUG] Fix a security issue when adding a project to a team. The tokens of the team owner were used instead of the user's.
* [BUG] Fix display issues in Firefox
* [BUG] Fix UI Cache issues
* [BUG] Offline projects color was not updated when pushing new dependency files
* [BUG] Sync was failing when commit already existed
* [BUG] Fix a bug preventing to upload new files in Offline projects

Known issues:

* [FEATURE] Gitlab support is delayed to beta3
* [BUG] Can't sign-in using an oauth account if the same email is already used

1.0.0-beta1 - 2016-10-21
------------------------

* First private beta
* GitHub.com and GitHub Enterprise support
* Slack integration for notifications
