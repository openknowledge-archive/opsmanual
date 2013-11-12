Resource and service monitoring
###############################

The Monitoring Service provides monitoring of servers and the
applications they serve (such as websites).

Resource monitoring
===================

-  `Munin <http://munin.okfn.org/>`__ on s001 (#321)

Availability monitoring
=======================

-  `Nagios3 <http://status.okfn.org/>`__ on s001 (#564)
-  Rackspace alert monitors (to be documented)

Service levels
==============

+--------+------------+-----------------------------------------------------------------------------+
| SL1    | low        | no backup/redundancy/alerting. But don't switch off without notice          |
+--------+------------+-----------------------------------------------------------------------------+
| SL2    | medium     | occasional backup, alerts but without immediate response, no redundancy     |
+--------+------------+-----------------------------------------------------------------------------+
| SL3    | high       | daily backups, 1-working-day response to alerts, running off redundant DB   |
+--------+------------+-----------------------------------------------------------------------------+
| SL3A   | high       | same a SL3/High, but with guaranteed response                               |
+--------+------------+-----------------------------------------------------------------------------+
| SL4    | critical   | fundemantal. ASAP response any time (e.g. central DB service)               |
+--------+------------+-----------------------------------------------------------------------------+

Availability history
====================

Nagios has an alert history and a simple report facility. In Nagios, go
to

-  "Host Detail" ==> select a host (e.g. "s060.okfn-openspending") ==>
   (top left corner of frame) "View Availability Report For This Host"
   ==> "Report period: This Year".

You can also request reports for any period. E.g. you want a report for
February. Convert start and end datetime into UNIX epoch (e.g.
`here <http://www.epochconverter.com/>`__), for example

-  1. Feb 2012 00:00 UTC = 1328054400
-  1. Mar 2012 00:00 UTC = 1330560000

and put them into this URL scheme:

-  https://$NAGIOS_CGI_BASE/avail.cgi?host=HOSTNAME&t1\ =$STARTTIME&t2=$ENDTIME

for example

-  http://status.okfn.org/cgi-bin/nagios3/avail.cgi?host=eu1&t1=1328054400&t2=1330560000

Setting up Munin
================

See also #321

-  Munin master:

   -  apt-get install munin
   -  symlink /etc/munin/munin.conf from sysadmin repo into /etc/munin)
   -  Update the aws firewall settings with ip address of munin master

-  Munin Node:

   -  apt-get install munin-node

      -  Install any relevent plugins
      -  Edit the munin-node conf to allow access from the munin master
         machine

Depricated monitors
===================

Don't use

-  Fry's `Munin monitor <https://monitor-okfn.fry-it.com/munin/>`__
-  Fry's `Nagios monitor <https://monitor-okfn.fry-it.com/nagios2/>`__
-  `HMG Nagios <http://nagios.hmg.ckan.net/>`__ on hmg.ckan.net
   monitoring the CKAN-HMG service group
-  Our `WasItUp <http://wasitup.com/>`__ account (e.g.
   {ca,de,www}.ckan.org, {blog,www}.okfn.org)
-  Montastic
-  http://isitup.org/ (See also #93)

Links
=====

-  http://ganglia.sourceforge.net/

.. raw:: mediawiki

   {{Category:Sysadmin}}
