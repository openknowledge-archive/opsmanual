Request Tracker
===============
We use Request Tracker (rt.okfn.org) as our ticketing system for a number of
services within Open Knowledge. This is a short overview of the current
sysadmin queues and how they're to be managed.

sysadmin-triage
---------------
All tickets come into sysadmin-triage (apart from the ones which go directly to
alerts). This queue should be checked on a regular basis and the tickets need
to be moved into the relevant queues below.

sysadmin
--------
Anything that's not Wordpress, Community or an Alert. So basically it's server
or software related.

sysadmin-alert
--------------
All Nagios alerts go into here. Currently they aren't auto-closed, but it's
something we're hoping to do.

sysadmin-wordpress
------------------
All wordpress related tickets go here.

sysadmin-community
------------------
All community issues go into here. Mostly end-user issues. If it's a server
issue, it should be in sysadmin.
