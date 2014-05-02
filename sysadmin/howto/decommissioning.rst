HOWTO: Decommissioning a server
===============================

Decommissioning a virtual or physical server is straightforward, but it is
important to follow a documented process to ensure that unnecessary alarms are
not raised and that important data is saved.

#.  Ensure that users are given appropriate notice.

    It is important that servers and services don't disappear unexpectedly if
    people rely upon them. Make sure that any users of the machine are given
    plenty of time to retrieve data or raise issues with the machine's
    decommissioning.

    Find out what services are hosted on the machine and make posts to
    appropriate mailing lists to announce the scheduled retirement of the
    machine.

#.  Take appropriate backups of machine state.

    Unless there is a good reason to do otherwise, it is probably a good idea to
    back up ``/etc``, ``/home``, ``/var/lib/{mysql,postgresql}`` and similar
    kinds of data. This is not an exhaustive list.

    Backups should usually be simple tarballs of the data in question, and
    should be uploaded to::

        s3://attic.okfn.org/archive/<hostname>-<backupname>-<yyyymmdd>.tar.gz

    For example::

        s3://attic.okfn.org/archive/s123.okserver.org-etc-20140401.tar.gz

#.  Schedule downtime for the machine in Nagios.

    This can be done using the Nagios user interface, or you can get Mowgli to
    do it. For example, to schedule ``s123.okserver.org`` for one day's
    downtime::

        !down s123 1d

#.  Power down the machine. The exact process to do this will be
    vendor-specific.

#.  Ensure that the right machine has gone offline according to Nagios. This
    sounds stupid, but do it!

#.  Destroy the machine. The exact process to do this will be vendor-specific.

#.  Remove the machine from the Ansible inventory.

#.  Run Ansible on the monitoring machine(s) to remove any monitoring for that
    host. In the ``infra`` repository::

        cd ansible/
        ./play -l monitoring main.yml

    This will force ``check_mk`` to regenerate its inventory and update Nagios.

#.  Remove any remaining entries relating to the machine in question from DNS.
