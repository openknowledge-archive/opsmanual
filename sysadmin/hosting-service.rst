''' IMPORTANT NOTE: PLEASE DO \*NOT\* CREATE A PERMANENT INSTANCE IF YOU
ARE NOT WILLING/ABLE TO PERFORM ALL THESE STEPS! '''

**In this case, please ask someone else to create the instance for you,
or ask for the necessary DNS/bitbucket/... credentials** (see section
*Access credentials* below). **If you intent to spin up a temporary
experimental instance, please mark it clearly as such and notify
sysadmin-coord.**

--------------

Remark: This page has been moved here from
http://trac.okfn.org/wiki/HostingService as per #992

--------------

How to spin up a new cloud instance
===================================

-  **Preparation:** Decide which hoster & datacenter the new server
   shall live in. Find the next free server number and set the new
   server's hostname according to [wiki:ServerNames].
-  **Deployment (Rackspace):** Spin up a new instance using Rackspace's
   management console (see remark below):

   -  Go to https://manage.rackspacecloud.com/
   -  Depending on whether you want to create a managed or unmanaged
      instance, log in either as user "okfn" or as "okfnmanaged"
      (request password at Rufus or Nils)
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

-  **DNS**: Add an A record for hostname and IP address, see
   `Sysadmin/DomainServices <Sysadmin/DomainServices>`__. You can check
   whether the record was created correctly with "" )
-  **Book keeping**: add the server information into the "Hosts" tab of
   our `Server/Service
   matrix <https://docs.google.com/spreadsheet/ccc?key=0Aon3JiuouxLUdC1IZ2kwRDMtX2ZaM0ZELWVJQzBrZXc#gid=11>`__.
-  **Customise**: The new cloud server has been deployed. Now customize
   it with our `fabric
   script <https://bitbucket.org/okfn/sysadmin/src/default/bin/fabfile.py>`__
   using its method "instance\_setup()". (On Amazon EC2, use user
   *ubuntu* instead of *root* in case of a EC2 instance). Set
   "``harden=False``\ " if you don't want to disable the root account
   and password login:

``   fab --host root@${HOST} instance_setup:hostname=${HOST},harden=True ``

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

-  **Monitoring:** Add the server to Nagios. On eu1.okfn.org:

   -  Add it to /etc/nagios3/conf.d/hosts.cfg
   -  In /etc/nagios3/conf.d/services\_nagios2.cfg add it to (and maybe
      )
   -  sudo /etc/init.d/nagios3 reload
   -  Check on http://status.okfn.org/cgi-bin/nagios3/status.cgi
   -  Don't forget to commit & push.

Remarks
-------

-  We now have "AWS Premium Support, Bronze." ~~As long as we do not
   subscribe to their "AWS Premium Support" our only way to file a
   technical issue report at Amazon is `this
   form <http://www.amazon.com/gp/html-forms-controller/aws-report-issue1>`__\ ~~
-  We do not yet have re-written our `aws
   scripts <https://bitbucket.org/okfn/sysadmin/src/tip/aws/>`__ to use
   `Rackspace's
   API <http://www.rackspace.com/cloud/cloud_hosting_products/servers/api/>`__
   (e.g. using `libcloud <http://incubator.apache.org/libcloud/>`__, see
   #345). Hence we use Rackspace's webinterface.
-  Rackspace has managed ubuntu servers only in their US cloud. That is
   about to change.
-  We can't file a set of standard changes to be applied to any new
   server; But we probably can declare one server to be the image master
   for new ones. See
   `RS#113872 <https://manage.rackspacecloud.com/Tickets/ViewTicket.do?ticketId=113872>`__
-  Rackspace does not have a concept similar to Amazon's EBS. On the
   other side it doesn't need it so badly because the images have
   reasonable size and are always persistant.
-  Rackspace does have an equivalent to S3 called
   "`Cloudfiles <http://www.rackspace.com/cloud/cloud_hosting_products/files/api/>`__\ ".
   They seem to be easy to use and would be the way to go if the
   persistent root block device is not large enough. Our Rackspace
   accounts are not yet activated to use Cloudfiles.
-  It doesn't look like there would be an equivalent to EC's "security
   groups", the Could Servers have to do their firewalling themselves.
   Exception: if we happen to use the services from Rackspace's
   "Dedicated Group", in which case there was a managed firewall in
   front of our cloud servers.
-  These are all virtualized boxes so you can't run VMs within them! See
   http://serverfault.com/questions/128145/linux-virtualization-options-on-ec2
   (comment from Nils: there should be ways to make OpenVZ/LXC/qemu
   work, but it's probably not worth the effort)

Access credentials
==================

-  Rackspace: Get username/password for their `management
   interface <https://manage.rackspacecloud.com/>`__ from Rufus
-  Amazon: There is a master password for the `AWS
   console <https://console.aws.amazon.com/ec2/home>`__. For `scripting
   AWS <http://docs.amazonwebservices.com/AWSSecurityCredentials/1.0/AboutAWSCredentials.html>`__,
   you need:

   -  For Amazon's REST API, e.g. our boto based
      **`aws/manage.py <https://bitbucket.org/okfn/sysadmin/src/default/aws/manage.py>`__**:
      Get the "AWS access key id" and the "AWS secret access key" from
      Rufus or from our AWS account. Create a ~/.boto file like this:

``  [Credentials]aws_access_key_id = ...aws_secret_access_key = ...``

-

   -  For Amazon's SOAP API, e.g. **ec2-tools** and scripts based on
      them: Get our AWS certificate and key PEM files from Rufus (SHA1
      Fingerprint=CC:8A:97:1A:10:BF:76:91:6C:A0:EF:D3:C1:F9:EC:C9:5D:1D:E3:AE).
      Store them on your local machine and export their locations before
      running the scripts:

| `` export EC2_CERT=${PATH_OF_AWS_CERTIFICATE_PEM_FILE}``
| `` export EC2_PRIVATE_KEY=${PATH_OF_AWS_KEY_PEM_FILE}``

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
