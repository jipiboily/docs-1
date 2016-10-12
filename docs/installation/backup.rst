Data Backup
===========

Gemnasium Enterprise will store its data in a persistent volume (cf. :ref:`docker_image_volumes`.).

It is YOUR responsibility to backup this volume. Gemnasium Enterprise has no capacities of backing up data.

Snapshots
---------

Disk snapshots are probably the best option to backup your data. All the data
stored in the persistent volume will saved at once, and can be restored at
anytime.

If a restore is needed, remember to stop the container before restoring anything::

    docker stop gemnasium

and remove it completely::

    docker rm gemnasium

Once the data is restored, run again the image: :ref:`run_docker_image`

Using docker
------------

As explained in the `"Manage data in containers" docker page <https://docs.docker.com/engine/tutorials/dockervolumes/#/backup-restore-or-migrate-data-volumes>`_, docker may be used to create a tarball:

    docker run --rm --volumes-from gemnasium -v $(pwd):/backup ubuntu tar cvf /backup/backup.tar /var/opt/gemnasium

It is highly recommanded to stop the container before doing this operation.

Restoring data with docker
^^^^^^^^^^^^^^^^^^^^^^^^^^

With your backup in the local directly (pwd), untar your archive to restore the data in the ``gemnasium-data`` volume::

    $ docker run --rm -v $(pwd):/backup -v gemnasium-data:/var/opt/gemnasium/ gemnasium/enterprise bash -c "cd /var/opt/gemnasium && tar xvf /backup/backup.tar --strip 1"

Once the data is restored, run again the image: :ref:`run_docker_image`
