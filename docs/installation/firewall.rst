Firewall configuration
======================

Gemnasium Enterprise is designed to run behind your firewalls, inside your own network.
It should be completely isolated from the outside, especially for incoming connections.

Incoming traffic
----------------

Gemnasium is just exposing 2 ports:

========================  ========================  ================
Port                      Usage                     protocol
========================  ========================  ================
80                        clear connection          http
443                       secure connection         https
========================  ========================  ================

It's your responsibility to configure your network and firewall to restrict access to these ports.
By default, Gemnasium will just expose the port 80 to redirect on the port 443. Custom ports might be used, refer to :ref:`ssl_configuration` if needed.

Outgoing traffic
----------------

While Gemnasium Enterprise should completely isolated from the outside for incoming connections, some outgoing traffic is required for normal operation.
The following ports must be open on your firewall for outgoing traffic:

========================  ========================  ====== ==========================================
Address                   port                      proto  Usage
========================  ========================  ====== ==========================================
sync.gemnasium.com        443                       tcp    Sync with Gemnasium main DB
index.docker.io           443                       tcp    Pull updates of gemnasium/enterprise image
========================  ========================  ====== ==========================================

What data is sent to sync.gemnasium.com?
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Your content is private, and will remain private. Anyway, synchronizing all packages, versions, changelogs, advisosories, etc. with
the whole main DB would require a huge storage, and a lot of bandwidth.

To avoid this situation, your instance of Gemnasium Enterprise will send a list of packages (dependencies) used in your projects to https://sync.gemnasium.com
In return, Gemnasium syncer will provide all the meta corresponding to this request, including advisories.

.. note:: Private dependencies will be completely ignored by the syncer.

Advisories can be created at any time on Gemnasium.com, that's why your instance will synchronize every five minutes.
If one of your project is using a dependency affected by a security issue, you will be instantly notified by your instance.

.. note:: Gemnasium is using data (advisories) from security companies (partnerships only). Your data will never be submitted directly to these companies, but Gemnasium may provide anonymous statitics.
