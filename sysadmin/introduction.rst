OKFN is operates many different services for developers, journalists,
policy makers, end users etc. Most are public, a few internal. Most are
web services. There are backends and frontends, databases, mail
services, DNS etc. See `Sysadmin/Services <Sysadmin/Services>`__ for an
overview of OKFN services. Most OKFN services run on servers operated by
OKFN. See `Sysadmin/HostingService <Sysadmin/HostingService>`__ on how
to set up a new server. Productive services run on managed servers at
`Rackspace <http://www.rackspace.com/>`__. Less critical services (e.g.
development, beta, testing) run at RackSpace, Amazon EC2 or Hetzner
machines. We are no longer running servers at Fry.

Productive services
===================

Induction
~~~~~~~~~

If you are working e.g. as sysadmin or developer or designer on a OKFN
service which is running on a managed server at Fry, please **make
yourself known to the OKFN sysadmin team** (see `Sysadmin <Sysadmin>`__
for contact details). Tell us what services you are working on and about
your role in the project.

There are several resources at you should have access to. Please contact
the sysadmin team so we create accounts for you:

-  Our `OKFN trac <http://trac.okfn.org>`__ to track issues.
-  Our monitors, see
   `Sysadmin/Monitoring\_Service <Sysadmin/Monitoring_Service>`__
-  If you need to know when servers undergo maintenance or get rebooted
   or major outages happen, you need to be subscribed to our mailing
   list <`sysadmin-announce at
   lists.okfn.org <http://lists.okfn.org/mailman/listinfo/sysadmin-announce>`__\ >.
   Tell us.
-  If you are deploying/operating whole servers, you would need access
   to our Rackspace accounts, see
   `Sysadmin/HostingService <Sysadmin/HostingService>`__.

**Sysadmin team responsibilities (scope)**
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  The sysadmin team is responsible for maintaining, monitoring, or
   setting up infrastructure and services hosted for OKFN.
-  The sysadmin team is expected to document and maintain the sysadmin
   space on the okfn wiki.
-  Help from the sysadmin team can be requested to setup, monitor,
   maintain servers or services.
-  The team provides support mainly via email.
-  Upon request the team helps other OKFN related projects, who may not
   host services along with core OKFN services.

Policies for maintaining, launching and closing services
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Productive services on managed servers should be monitored by Rackspace,
other services might be monitored by ourselves. If a service-down alert
is triggered, some sysadmin will investigate and maybe restart the
service or reboot the server. Therefore there are some rules:

-  When you work on a services and there is a risk that it might become
   unavailable or unresponsive,

   -  If the service is monitored by Rackspace, in theory we should
      disable this monitor temporarily in the Rackspace control panel.
      Unfortunately that is quite clumsy to do and would require
      privileged access to their control panel. So only do that if a
      largish downtime is expected and otherwise try to be quick and to
      not trigger an alarm ...

-  If you close down a service, notify the sysadmin team and ask them to
   remove any monitors
-  If you plan to setup a new service, please speak to the sysadmin
   team, or just file a ticket according to
   `NewService <http://trac.okfn.org/wiki/NewService>`__.
-  When you launch the new productive service, either setup a Rackspace
   monitor yourself (if you have access to their control panel), as ask
   the sysadmin team to do it for you.

**Server Access**
~~~~~~~~~~~~~~~~~

-  The current way to access servers is by having your pub key added
   into the **okfn** user.
-  This method of access needs to change, we need to have multiple users
   with sudo su - disabled.

Hetzner bare metal machines for non-critical purposes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

We have some fast bare-metal machines at
`Hetzner <http://hetzner.de/>`__ (at the time of this writing s030-s033
and s110). They are for **non-critical purposes** only, e.g. for
development or testing. They are not suitable to run critical (e.g.
productive) services on.

Note that these servers are provided as-is with **no backup, minimal
monitoring and no disaster recovery**. Consider the Hetzner machines
server and all their data **ephemeral**, they might vanish at any time
**without warning**!

Other things you should know
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

-  Rackspace monitoring alerts go to . Rackspace will try to solve, if
   they can't they'll escalate the issue to the OKF sysadmin team, and
   we might escalate it to you.
-  Rackspace machines can be resized to have more memory or disk space.
   Resizing requires a reboot.
-  You might want to read about our
   `Sysadmin/DomainServices <Sysadmin/DomainServices>`__. Please `file a
   ticket <http://trac.okfn.org/newticket?component=sysadmin>`__ if you
   need DNS records to be set/changed for domains which OKF holds..

Non-critical services
=====================

(To be done)

Notes
~~~~~

These notes need to get turned into proper documentation:

-  /etc is versioned (nowadays in a mercurial repository - was in a svn
   repo). This allows to log sysadmin activity (*hg log*) and tracking,
   reversion, and exporting of changes. Our `fabric
   command <https://bitbucket.org/okfn/sysadmin/src/default/bin/fabfile.py>`__
   automates setting this up.
-  Config files get symlinked from /etc/ to /home/okfn/etc/ (which
   itself is a symlink to /home/okfn/hg-sysadmin/etc). E.g.

``ln -s /home/okfn/etc/apache2/* /etc/apache2/sites-available``

-  Apps/services get deployed to /home/okfn/var/srvc/{domain.name} (or,
   alternatively, deployed to a dedicated user account /home/{xxx} for
   that app
