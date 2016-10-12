Firewall configuration
======================

Gemnasium Enterprise is designed to run behind your firewalls, inside your network.
It should be completely isolated from the outside, especially from incoming connections.

Incoming traffic
----------------

Gemnasium only exposes two ports:

========================  ========================  ================
Port                      Usage                     Protocol
========================  ========================  ================
80                        clear connection          http
443                       secure connection         https
========================  ========================  ================

It's your responsibility to configure your network and firewall to restrict access to these ports.
By default, Gemnasium exposes port 80 only to redirect to port 443. Custom ports might be used, refer to :ref:`ssl_configuration` if needed.

Outgoing traffic
----------------

While Gemnasium Enterprise should be completely isolated from the outside for incoming connections, some outgoing traffic is necessary for normal operation.
The following ports must be open on your firewall for outgoing traffic:

========================  ========================  ========= ==========================================
Address                   Port                      Protocol  Usage
========================  ========================  ========= ==========================================
sync.gemnasium.com        443                       tcp       Sync with Gemnasium main DB
index.docker.io           443                       tcp       Pull updates of gemnasium/enterprise image
========================  ========================  ========= ==========================================

What data is sent to sync.gemnasium.com?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Your content is private, and will remain private. But synchronizing all packages, versions, changelogs, advisories, etc. with
Gemnasium's main DB would require a huge amount of storage and a lot of bandwidth.

To avoid this situation, your instance of Gemnasium Enterprise periodically sends a list of the packages (dependencies) used in your projects to https://sync.gemnasium.com
In return, Gemnasium syncer provides all the metadata corresponding to this request, including the security advisories.

.. note:: Private dependencies are completely ignored by the syncer.

Advisories can be created at any time on Gemnasium.com; that's why your instance synchronizes hourly.
If one of your projects is using a dependency affected by a security issue, you will be instantly notified by your instance.

.. note:: Gemnasium is using data (advisories) from security companies (partnerships only). Your data will never be submitted directly to these companies, but Gemnasium may share anonymized statistical data.
