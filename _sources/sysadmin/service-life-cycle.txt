Service lifecycle
=================

.. todo:: This document is out-of-date and needs updating.

This document describes the process of requesting a server/service and its
life cycle.

Prerequisites of requesting a Server or Service
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Before requesting for a service, please make sure the below set of
   details are available.

   -  New server or service requests should be sent in by project leads,
      and or with approval.
   -  SLA and point of contact incase of emergencies should be provided
      by the project lead.

Setting up/requesting a new service or server
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  So if are about to deploy a new service at OKFN or want the sysadmin
   team to deploy it,

   -  Fill up the google form `server request
      form <https://docs.google.com/spreadsheet/viewform?formkey=dC1IZ2kwRDMtX2ZaM0ZELWVJQzBrZXc6MQ>`__
   -  Mentioned below are the set of fields which needs to be filled up.

| ``Timestamp``
| ``Org Unit``
| ``Project``
| ``Name of service``
| ``Description``
| ``Maintainer Name``
| ``Maintainer Email``
| ``Type of service needed``
| ``required software``
| ``Does this service require its own dedicated server? why?``
| ``service level``
| ``backup required?``
| ``Monitoring required?``
| ``Your name``
| ``ETA``
| ``Who is expected to deploy this service/server``
| ``what sort of access to the server/service is required by the maintainer?``

-  Once the form is submitted, please ensure you send a mail to
   sysadmin@ okfn.org notifying the sysadmins, you've submitted the
   request.
-  This will help us update our records as well, `service
   matrix <https://spreadsheets.google.com/ccc?key=t-Hgi0D3-_fZ3FD-eIC0kew#gid=11>`__.

Remarks on hostnames
~~~~~~~~~~~~~~~~~~~~

-  Every server has a hostname like s001.okserver.org with its A record.
-  You will need to ensure the hostname isn't already in use by looking
   at the the server matrix document - 'hosts' tab.
-  A records for a domain may be added via the `DME
   interface <http://wiki.okfn.org/Sysadmin/DomainServices>`__

-  **Only introduce aliases if you have a very good reason to do so**

   -  For non-production services please only use one of these
      prefixes/suffixes:
   -  *staging* - pre-release software testing, don't use: alpha, beta,
      test, testing
   -  *demos* - client-facing non-production services. don't use: demo,
      uat
   -  *dev* - development services. don't use: devel, hack, (buildbot)
   -  "sandbox" - public permanent non-production service.

Decomission/EOL servers or service
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Notify sysadmin-announce
-  Terminate service/server
-  Mark with date and "sign-off $NAME" as "closed" or "terminated in
   service matrix
-  (if server) remove from munin
-  remove from nagios
-  either remove from DNS and squid.con, or redirect in squid/nginx to
   other service or to a "This service is decommissioned" page
