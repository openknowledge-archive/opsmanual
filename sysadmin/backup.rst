Backups for our services: state of the union
############################################

.. todo:: This is hopelessly out of date and FAR too detailed (and thus prone to
          going out of date). Needs fixing.

Overview
========

At Rackspace, we have two methods of backup:

-  **Snapshotting** of the VMs. Should be activated for **all** VMs.
   Important Notes:

   -  The maximum image size that can be snapshotted at RS is **80GB**.
      If a VM disk contains more data, it cannot be snapshotted!
   -  There is a bug in the rackspace interface that disables the
      snapshot schedule when a VM is resized. Make sure you re-enable it
      after resizing a VM!
   -  The retention is.

      -  Managed VMs: 14 daily images and 1 weekly image
      -  Unmanaged VMs: 2 daily images and 1 weekly image

-  **File-level backup** is **only** available for managed VMs. One can
   configure which directories to backup.

Setting up backups ...
======================

... at Rackspace
----------------

The VMs should already be snapshotted (*Hosting* ==> *Cloud Servers* ==>
*My server Images*). If you need file-level backup and the VM is a
managed one, then you can activate and configure Rackspace Backup quite
easily in the Rackspace control panel.

-  When using the https://mycloud.rackspace.com insterface to deploy a
   cloud server, please ensure you create a first generation instance
   (under the region drop down), next generation instances do not
   support periodic snapshots(as of Dec 2012)

Backup status outside Rackspace
===============================

For a list of backup status per host, see the *"backup"* column in our
`host
matrix <https://docs.google.com/spreadsheet/pub?key=0Aon3JiuouxLUdC1IZ2kwRDMtX2ZaM0ZELWVJQzBrZXc&single=true&gid=14&output=html>`__

-  Bytemark (s001)

   -  Essetial data pulled nightly to s088/rsnapshot

-  Amazon

   -  Those EC2 VMs that run off "instance-store" have a EBS device
      attached as /dev/sdp and backup into that daily (4 day retension)
   -  All EBS volumes get manually snapshotted, roughly monthly.

-  Linode (s034, s035)

   -  snapshotted nightly, similar to Rackspace

Disaster recovery
=================

At Rackspace it should be simple and straightforward: just re-create the
VM from its latest snapshot. For a managed VM we might even ask
Rackspace to do that for us.

Same should be true for Linode hosts.

For EBS-based EC2 hosts, one can try to recreate the VM from the latest
manual EBS-snapshot and go from there. See Appendix below

For all other machines, one had to re-install it and inject the data
from a backup (if any).

Appendix
========

Appendix: Backup issues
-----------------------

-  *Proper* backup should be either covered by an SLA (e.g. Rackspace,
   Bytemark managed), or at least not at the same provider.
-  Linode: snapshots might be too few. Linode's backup system does work
   on the fs layer, not on the block layer which makes it `pretty
   limited <https://www.linode.com/docs/platform/backup-service#limitations>`__.
-  Amazon EC2:

   -  Some EC2 servers are running off instance-store and have no
      /dev/sdp attached (bkn-2, eu11, eu15)
   -  EBS snapshotting is currently done manually.
   -  Our backup scripts at EC2 has issues:
   -  Doesn't cope with /dev/sdp being mounted elsewhere
   -  Lock file sometimes left over, no warning sent (?)
   -  Several similar but not equal scripts on eu2 to backup itself and
      eu1/us2
   -  Does only do (four) daily backups, no weekly or monthly.

Appendix: EC2: Setting up internal backup for instance-store VMs
----------------------------------------------------------------

For how to set up backups on AWS machines see browser:doc/BACKUP.txt

Appendix: EC2: Create EBS-based instance from a snapshot
--------------------------------------------------------

You will need the necessary credentials for Amazon's SOAP API (your AWS
certificate and key PEM files) in order to use Amazon's ec2-tools, see
section *Access credentials* in [wiki:HostingService].

There should be at least monthly snapshots from our EBS volumes. In
order to restore an instance from a snapshot you need this information:

-  The architecture, "i386" or "x86\_64".
-  The id of the snapshot (e.g. "snap-0ab6ed63"). Must be compatible
   with the architecture.
-  The `instance type <http://aws.amazon.com/ec2/instance-types/>`__ of
   the machine (e.g "t1.micro"). Must be compatible with the
   architecture.
-  The id of the kernel (and ramdisk if any) to be used. I guess it does
   not need to be exactly the same as the one of the original machine,
   it only needs to meet the architecture and the distribution, (e.g.
   the ubuntu lucid x86\_64 kernel "aki-14340160"). Look into our
   `manage.py <https://bitbucket.org/okfn/sysadmin/src/default/aws/manage.py>`__
   script for hints of which kernel id was probably used for the
   original instance.

Now when you have all the information, do two steps: Create a system
image from the snapshot, and deploy a new instance from that system
image.

| ``ec2-register --region eu-west-1 --architecture x86_64 --snapshot snap-0ab6ed63 --kernel aki-14340160 --name 'Image_eu17_backup_snapshot'``
| ``IMAGE  ami-3cdaec48``
| ``ec2-run-instances --region eu-west-1 ami-3cdaec48 --availability-zone eu-west-1b --kernel aki-14340160 --instance-type t1.micro``

Congratulation, you now have cloned a EC2 instance from a snapshot!

**Warning:** This procedure does \*not\* yet consider

-  membership of the new instance in security groups
-  instance name tag (see section *Deployment* in [wiki:HostingService])
-  setting the "disable-api-termination" flag (see section *Deployment*
   in [wiki:HostingService])

Appendix: Linode snapshots
--------------------------

Note: Beware of Linode's `backup
restrictions <https://www.linode.com/docs/platform/backup-service#limitations>`__.
OK, this is how it works (as of 2011-03-25):

-  Log into https://manager.linode.com/
-  If you want to recover to not the same Linode, create a new Linode:

   -  At least the same size and at the same location as the
      to-be-cloned Linode
   -  When asked which Linux distribution to install, either
      abort/navigate back, or select some random distro and later remove
      all disk images.

-  Make sure the Linode you want to restore \*to\* has enough
   un-allocated disk-allowance (that is different from "free disk
   space"). In necessary, remove disk images.
-  Go into the Dashboard of the to-be-cloned Linode, tab *Backups*
-  Select the backup snapshot to recover from and press *restore to...*.
   There now should be a Linode with enough free disk-allowance to
   restore to.
