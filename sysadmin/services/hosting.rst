Server hosting
##############

''' IMPORTANT NOTE: PLEASE DO \*NOT\* CREATE A PERMANENT INSTANCE IF YOU
ARE NOT WILLING/ABLE TO PERFORM ALL THESE STEPS! '''

**In this case, please ask someone else to create the instance for you,
or ask for the necessary DNS/bitbucket/... credentials** (see section
*Access credentials* below). **If you intent to spin up a temporary
experimental instance, please mark it clearly as such and notify
sysadmin-coord.**


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

-  **Bootstrap**: Bootstrap with Ansible. TODO

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

-  **Monitoring:** Done with ansible now. Mention how to do it


Access credentials
==================

Credentials for the web interface should be in LastPass. Private keys and
other shared files should be in the `okfn/credetials
<https://github.com/okfn/credentials>` repo.

Appendix: Hosting costs
=======================

Rackspace hosting costs
-----------------------

Rackspace has several datacenters for cloud servers. At least one dc is
in UK (London). The US and the UK prices differ.

-  Linux VMs are paid by memory per time: 6ct (4p) per GB\*h = $43.83
   (£29.22) per GB\*month
-  Windows (2008) VMs are : 8ct (5.2p) per GB\*h = $58.40 (£37.96) per
   GB\*month
-  managed VMs cost a premium: 12ct (10p) per h = $87.66 ($73.05) per
   month per VM
-  There is a fixed monthly account fee if there is at least one managed
   VM: $100 (£65) per month
-  There is no charge for incoming traffic
-  There is a charge for ourgoing traffic: 18ct (12p) per GB = $180
   (£120) per TB

So the formula for the monthly fee is:

-  US cloud servers:

``   $43.83/GB  *  total memory of all Linux VMs +   $58.40/GB  *  total memory of all Windows VMs +   $87.66     *  total number of managed VMs  +  $100.00        (if there is at least 1 managed VM) +  $180.00/TB  *  total outgoing traffic``

-  UK cloud servers:

``   £29.22/GB  *  total memory of all Linux VMs +   £37.96/GB  *  total memory of all Windows VMs +   £73.05     *  total number of managed VMs +   £65.00        (if there is at least 1 managed VM) +  £120.00/TB  *  total outgoing traffic``

Remarks:

-  New features are first rolled out in the US cloud.
-  Managed and unmanaged cloud servers can only be adminstered with
   separate Rackspace accounts. :-(
-  For my customers, traffic costs have been neglectable so far.

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

Should go into our Fabric script (see #601):

-  Disable too strict fail2ban rule which blocks ssh login from IPs with
   broken reverse DNS:

| ``   sudo cp -a /etc/fail2ban/filter.d/sshd.conf /etc/fail2ban/filter.d/sshd.conf.ORIG``
| ``   sudo sed '/POSSIBLE BREAK-IN ATTEMPT/d' -i /etc/fail2ban/filter.d/sshd.conf``
| ``   sudo /etc/init.d/fail2ban restart    ``

-  If you disabled ssh password login, add an exception for the
   Rackspace management IPs:

| ``   sudo cp -a /etc/ssh/sshd_config /etc/ssh/sshd_config.1``
| ``   echo -e "\nMatch Host 174.143.23.0/25,94.236.100.0/25,89.234.31.0/25,64.39.4.132\n    PasswordAuthentication yes" | \``
| ``       sudo tee -a /etc/ssh/sshd_config    sudo restart ssh``
