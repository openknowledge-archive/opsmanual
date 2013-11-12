Amazon Web Services (EC2 etc)
=============================

Useful Info
-----------

-  `Running user provided script post boot of an
   AMI <http://alestic.com/2009/06/ec2-user-data-scripts%20>`__

EC2 Details
-----------

References
~~~~~~~~~~

-  `Running MySQL on Amazon EC2 with Elastic Block
   Store <http://developer.amazonwebservices.com/connect/entry.jspa?externalID=1663>`__
-  `Persistent Django on Amazon EC2 and EBS ‚Äì The easy
   way <http://thomas.broxrost.com/2008/08/21/persistent-django-on-amazon-ec2-and-ebs-the-easy-way/>`__

Installation
~~~~~~~~~~~~

| ``apt-get install mysql-server postgresql    ``
| ``Mysql root password: *****  ``

EBS setup
~~~~~~~~~

-  We have one 60GB disk which is attached at */dev/sdf* through the
   Amazon console.

MySQL configuration
~~~~~~~~~~~~~~~~~~~

| ``/etc/init.d/mysql stop``
| ``mkdir /mnt/backup/lib /mnt/backup/log /mnt/backup/etc``
| ``mv /etc/mysql /mnt/backup/etc``
| ``mv /var/lib/mysql /mnt/backup/lib/``

| ``mv /var/log/mysql  /mnt/backup/log``
| ``mkdir /etc/mysql``
| ``mkdir /var/lib/mysql``
| ``mkdir /var/log/mysql``
| ``echo "/mnt/backup/etc/mysql /etc/mysql     none bind" | tee -a /etc/fstab``
| ``echo "/mnt/backup/lib/mysql /var/lib/mysql     none bind" | tee -a /etc/fstab    ``
| ``echo "/mnt/backup/log/mysql /var/log/mysql     ``
| ``none bind" | tee -a /etc/fstab       ``
| ``mount /etc/mysql/   ``
| ``mount /var/lib/mysql   ``
| ``mount /var/log/mysql      ``
| ``/etc/init.d/mysql start ``

Postgresql Configuration
~~~~~~~~~~~~~~~~~~~~~~~~

| ``/etc/init.d/postgresql-8.3 stop       mv /var/lib/postgresql /mnt/backup/lib   ``
| ``mv /var/log/postgresql/ /mnt/backup/log   ``
| ``mv /etc/postgresql /mnt/backup/etc/      ``
| ``mkdir /var/lib/postgresql   ``
| ``mkdir /var/log/postgresql   ``
| ``mkdir /etc/postgresql      ``
| ``echo "/mnt/backup/etc/postgresql /etc/postgresql     none bind" | tee -a /etc/fstab   ``
| ``echo "/mnt/backup/lib/postgresql /var/lib/postgresql     none bind" | tee -a /etc/fstab   ``
| ``echo "/mnt/backup/log/postgresql /var/log/postgresql     none bind" | tee -a /etc/fstab      ``
| ``mount /etc/postgresql ``
| ``mount /var/lib/postgresql/   ``
| ``mount /var/log/postgresql/      ``
| ``/etc/init.d/postgresql-8.3 start     ``

Apache
~~~~~~

| ``/etc/init.d/apache2 stop``
| ``mv /var/log/apache2 /mnt/backup/log/``
| ``mv /etc/apache2/ /mnt/backup/etc/``
| ``mkdir /etc/apache2   mkdir /var/log/apache2 ``
| ``echo "/mnt/backup/etc/apache2 /etc/apache2     none bind" | tee -a /etc/fstab``
| ``echo "/mnt/backup/log/apache2 /var/log/apache2     none bind" | tee -a /etc/fstab``
| ``/etc/init.d/apache2 start``

KForge
~~~~~~

TODO

Changes that can be lost
~~~~~~~~~~~~~~~~~~~~~~~~

-  Packages installed - there is a cron job that dumps the package list.
   \* Reload with dpkg --set-selections filename && apt-get install
-  Attached EBS devices - reattach through console, /etc/fstab in full:

| ``# Legacy /etc/fstab  # Supplied by: ec2-ami-tools-1.3-21885 ``
| ``/dev/sda1 /     ext3    defaults 1 1 ``
| ``/dev/sda2 /mnt  ext3    defaults 0 0 ``
| ``/dev/sda3 swap  swap    defaults 0 0 ``
| ``none      /proc proc    defaults 0 0 ``
| ``none      /sys  sysfs   defaults 0 0  # # these bits are the bits that would need to be restored...  # /dev/sdf /mnt/backup    defaults 0 0``
| ``/mnt/backup/etc/mysql /etc/mysql     ``
| ``none bind /mnt/backup/lib/mysql /var/lib/mysql     ``
| ``none bind /mnt/backup/log/mysql /var/log/mysql     ``
| ``none bind /mnt/backup/etc/postgresql /etc/postgresql     ``
| ``none bind /mnt/backup/lib/postgresql /var/lib/postgresql     ``
| ``none bind /mnt/backup/log/postgresql /var/log/postgresql     ``
| ``none bind /mnt/backup/etc/apache2 /etc/apache2     ``
| ``none bind /mnt/backup/log/apache2 /var/log/apache2  none bind``
