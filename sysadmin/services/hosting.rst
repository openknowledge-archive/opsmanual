Server hosting and building new server
######################################

How to spin up a new cloud instance
===================================

-  **Preparation:** Decide which hoster & datacenter the new server
   shall live in. Find the next free server number and set the new
   server's hostname.
-  **Deployment (Rackspace):** Spin up a new instance using Rackspace's
   management console (see remark below):

   -  Go to https://manage.rackspacecloud.com/
   -  Depending on whether you want to create a managed or unmanaged
      instance, log in either as user "okfn" or as "okfnmanaged".
   -  Go to "Hosting/Cloud Servers", tab "Server Instances". Press "Add
      Server".
   -  Select "Ubuntu 12.04 LTS" and enter hostname and amount of RAM.
   -  Rackspace will display you the details of the server, in
      particular its IP address.
   -  Once the system has been built and booted, a mail with a random
      root password will go to sysadmin@okfn.org.

-  **Deployment (Amazon EC2):** Spin up a new instance using our AWS
   script, add a name tag and enable the "termination protection" (yes,
   the latter will evetually be handled by manage.py too). See also
   section *Access credentials* below.

| ``   cd sysadmin/aws/    ./manage.py  create_instance --region eu-west-1 ubuntu-lucid-64-ebs t1.micro eu17``
| ``   ec2-create-tags --region eu-west-1 $INSTANCE --tag Name=eu17 ``
| ``   ec2-modify-instance-attribute --region eu-west-1 --disable-api-termination true $INSTANCE``

-  **DNS**: Add an A record for hostname and IP address.

-  **Bootstrap**: To bootstrap the machine, use bootstrap.yml in the `infra`
  repository::

    ./play bootstrap.yml --extra-vars="hostip=<newip> host=<newhostname>
    user=ubuntu" -v -s

-  '''Reverse DNS (not for Amazon) ''': Set server hostname as reverse
   DNS for the server's main IP address.

   -  RackSpace: In the Rackspace management interface's overview for
      this server, click on the tab "DNS". In section "Reverse DNS
      Management", set the hostname as reverse DNS record for the IP
      address.

-  **Backup:** (optional, Rackspace only) If the new host shall be
   automatically snapshot: Within Rackspace's management interface's
   overview for this server ...

   -  Click on the tab "Images" and press "Enable scheduled Imaging"
   -  Select e.g. daily backup 02:00-04:00 and weekly backup on Sundays
   -  Finish with "Save Schedule".

-  **Monitoring:** Adding the machine to the respective host group for
   monitoring.


Access credentials
==================

Credentials for the web interface should be in LastPass. Private keys and
other shared files should be in the `okfn/credetials
<https://github.com/okfn/credentials>` repo.

Appendix: Hosting costs
=======================

Rackspace hosting costs
-----------------------

The formula for the monthly fee is:

-  US cloud servers:

``   $43.83/GB  *  total memory of all Linux VMs +   $58.40/GB  *  total memory of all Windows VMs +   $87.66     *  total number of managed VMs  +  $100.00        (if there is at least 1 managed VM) +  $180.00/TB  *  total outgoing traffic``

-  UK cloud servers:

``   £29.22/GB  *  total memory of all Linux VMs +   £37.96/GB  *  total memory of all Windows VMs +   £73.05     *  total number of managed VMs +   £65.00        (if there is at least 1 managed VM) +  £120.00/TB  *  total outgoing traffic``

Sources
-------

-  RackSpace:

   -  `US cloud
      prices <http://www.rackspace.com/cloud/cloud_hosting_products/servers/pricing/>`__
   -  `UK unmanaged
      cloud <http://www.rackspace.co.uk/cloud-hosting/cloud-products/cloud-servers/prices/>`__
   -  `UK managed
      cloud <http://www.rackspace.co.uk/cloud-hosting/cloud-products/managed-cloud/prices/>`__

-  Memset:

   -  By the hour (unmanaged only): http://www.memset.com/cloud/compute/
   -  Monthly: http://www.memset.com/dedicated-servers/vps/
   -  Managed: http://www.memset.com/support/self-managed.php

-  Linode: https://manager.linode.com/signup/#plans
-  Amazon EC2:

   -  Types: http://aws.amazon.com/ec2/#instance \* Pricing:
      http://aws.amazon.com/ec2/pricing/
   -  Calculator: http://calculator.s3.amazonaws.com/calc5.html

Appendix: Standard changes to managed Rackspace servers
=======================================================

-  If you disabled ssh password login, add an exception for the
   Rackspace management IPs:

| ``   sudo cp -a /etc/ssh/sshd_config /etc/ssh/sshd_config.1``
| ``   echo -e "\nMatch Host 174.143.23.0/25,94.236.100.0/25,89.234.31.0/25,64.39.4.132\n    PasswordAuthentication yes" | \``
| ``       sudo tee -a /etc/ssh/sshd_config    sudo restart ssh``
