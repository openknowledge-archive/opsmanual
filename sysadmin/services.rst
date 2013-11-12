(This page was originally at http://trac.okfn.org/wiki/Services and has
been move here as per `this ticket <http://trac.okfn.org/ticket/992>`__)

OKFN is operating a lot of services most of which run on OKFN-operated
servers ("hosts"). Most services are web applications ("apps"),
basically websites or apis used by end users. Some are infrastructure
services (like email, mailing lists, DNS) or backends (e.g. backup,
search, messaging, databases etc).

Lists of OKFN services and servers
----------------------------------

Current service lists (in approximate order of authorativeness):

-  `DNS records <https://dns.fry-it.com/>`__ (see
   `Sysadmin/DomainServices <Sysadmin/DomainServices>`__)
-  `http cache
   configuration <https://bitbucket.org/okfn/sysadmin/src/default/etc/squid3/squid.conf>`__
   (see `CacheService <http://trac.okfn.org/wiki/CacheService>`__)
-  `Host to Service
   Matrix <http://spreadsheets.google.com/ccc?key=t-Hgi0D3-_fZ3FD-eIC0kew>`__
   google spreadsheet
-  The avalability monitors: (see
   `Sysadmin/Monitoring\_Service <Sysadmin/Monitoring_Service>`__)

Documentation of core services
------------------------------

-  `Sysadmin/HostingService <Sysadmin/HostingService>`__
-  `Sysadmin/DomainServices <Sysadmin/DomainServices>`__
-  `Sysadmin/Monitoring\_Service <Sysadmin/Monitoring_Service>`__
-  `Sysadmin/Wordpress\_Service <Sysadmin/Wordpress_Service>`__
-  `Sysadmin/Database\_Service <Sysadmin/Database_Service>`__
-  `Sysadmin/Wiki\_Service <Sysadmin/Wiki_Service>`__
-  `CacheService <http://trac.okfn.org/wiki/CacheService>`__
-  `EmailService <http://trac.okfn.org/wiki/EmailService>`__
-  `FirewallService <http://trac.okfn.org/wiki/FirewallService>`__
-  `EtherpadService <http://trac.okfn.org/wiki/EtherpadService>`__
-  `ImageHostingService <http://trac.okfn.org/wiki/ImageHostingService>`__
-  `MailingListService <http://trac.okfn.org/wiki/MailingListService>`__
-  `MessagingService <http://trac.okfn.org/wiki/MessagingService>`__
-  `WebAnalyticsService <http://trac.okfn.org/wiki/WebAnalyticsService>`__

--------------

Historic notes, disregard
-------------------------

-  `JSON list of EC2
   servers <https://bitbucket.org/okfn/sysadmin/src/default/servers.js>`__
-  `CKAN community
   info <https://spreadsheets.google.com/spreadsheet/ccc?key=0AplklDf0nYxWdE1YMXllNGliREVFQ3k3WmpPaVJVWEE#gid=0>`__
   matrix (google docs)
-  manual check script
   `check\_okfn\_websites.py <https://bitbucket.org/okfn/sysadmin/src/default/bin/check_okfn_websites.py>`__
-  The script
   `check\_okfn\_websites2.sh <https://bitbucket.org/okfn/sysadmin/src/default/bin/check_okfn_websites2.sh>`__
   parses all nginx/apache/squid configurations for configured web
   service hostnames; DNS-Resolves all hostnames found, and matches them
   against the IP addresses of our servers; compares configured
   hostnames against their DNS mapping; throws warning on no-match;
   generates nagios config for matches.
-  See also our overview of `DGU/HMG
   services <https://trac.dataco.coi.gov.uk/projects/datagov/wiki/SysadminCkanDgu>`__
