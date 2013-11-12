OKF's central database servers
==============================

We currently have only one central, non-dedicated database servers:

-  **db-euw1.okserver.org** (s005.okserver.org aka eu5.okfn.org) has
   postgres and mysql in Amazon's eu-west-1 datacenter. It is accessible
   ...

   -  ... for clients in eu-west-1 on it's private IP address
      db-euw1-int.okserver.org. (aka psql.okfn.org = eu5-int.okfn);
   -  ... for other clients as db-euw1.okserver.org, but they have to be
      explicitly allowed in the server's EC2 "security group"
      instance-827de311-4c58-4328-9461-187af35342a7 (see also
      `Sysadmin/Firewall Service <Sysadmin/Firewall Service>`__)

**Important note**: please do create different DB user names and
passwords for each client application - do not use generic DB users like
"okfn". We do not want that an evildoer who managed to compromise our
webapp A automatically gains access to the DBs of webapps B, C, ... too!

--------------

(Examples removed as they were outdated, and we no longer use
non-dedicated DB servers for new webapps)
