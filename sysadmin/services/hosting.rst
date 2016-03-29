Server hosting and building new server
######################################

How to spin up a new cloud instance
===================================

-  **Preparation:** Decide which hoster & datacenter the new server shall live
  in. Find the next free server number and set the new server's hostname.
-  **Deployment (Amazon EC2):** Spin up a new instance using our AWS script,
  add a name tag and enable the "termination protection". See also section
  *Access credentials* below.

-  **DNS**: Add an A record for hostname and IP address.

-  **Bootstrap**: To bootstrap the machine, use bootstrap.yml in the `infra`
   repository::

     ansible-playbook bootstrap.yml --extra-vars="hostip=<newip> host=<newhostname> user=ubuntu" --verbose -s

-  **Monitoring:** Adding the machine to the respective host group for
   monitoring.


Access credentials
==================

Credentials for the web interface should be in LastPass. Private keys and other
shared files should be in the `okfn/credentials
<https://github.com/okfn/credentials>` repo.
