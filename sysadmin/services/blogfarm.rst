Blogfarm
########

OKF has a blogfarm at specialized Wordpress hoster `WPEngine
<http://wpengine.com>`__. It is a wordpress multisite installation that serves
numerous properties from a single codebase. It can provide sites at
**{name}.okblogfarm.org** as well as at any domain name (via Wordpress' domain
alias feature).

Amongst the sites it currently hosts are the `OKFN blog
<http://blog.okfn.org/>`__, local OKFN chapters like `OKFN Belgium
<http://okfn.be/>`__ or `OKFN Germany <http://okfn.de/>`__, the `Public Domain
Review <http://publicdomainreview.org/>`__, `CKAN <http://ckan.org/>`__, the
`Open Knowledge & Data Festival <http://okfestival.org/>`__, `OpenGLAM
<http://openglam.org/>`__, `Digitised Manuscripts to Europeana
<http://dm2e.eu/>`__ and many others.

WPEngine Overview
=================

Some basic instructions outlining the code deployment procedures for our
blog farm hoster WPE (WPEngine - http://wpengine.com/).

WPE code deployment is done through their git post-recieve hook. In
essence it's as simply as commiting code changes to the repository and
doing a git push back to the WPE remote. More detailed instructions can
be found in their documentation about this - http://git.wpengine.com/.

Access to the WPE git repos is controlled by public keys. You'll need to
contact the OKF Sysadmin team to have your public key granted access
before proceeding. Once this has been done, confirm access by::

    $ ssh git@git.wpengine.com info

You should get a response back listing the repositories you have access
to. At a minimum these should be::

    production/okf
    staging/okf

Consider these as two separate git remotes containing the code for the
production web dir and staging web dir respectively.

Git Workflow
============

We maintain the code on our github repository. ::

    $ git clone git@github.com:okfn/wordpress.git wordpress
    $ cd wordpress

This repository uses submodules for the various plugins and themes. So
you should initialize and update these::

    $ git submodule update --init

The recommended workflow is to add remotes for production and staging::

    $ git remote add staging git@git.wpengine.com:staging/okf.git
    $ git remote add prod git@git.wpengine.com:production/okf.git

The wordpress repository has two branches, master and prod. Changes need to
land in master, be pushed to staging, and then merged into the prod branch.

Deploying changes is as simple as pushing your commits back to the
staging or production. So something like the following. It's advised that
you do this via a terminal window so that you can see descriptive
deployment progress output, along with any custom errors. When you push to
staging, it's recommended that you push to origin at the same time so we keep
our github repo up-to-date::

    $ git commit
    $ git push staging master
    $ git push origin master

To push to production, switch to the prod branch, merge master into it, and
push to prod::

    $ git checkout prod
    $ git merge master
    $ git push prod prod
    $ git push origin prod

Sub Modules
-----------

Note that submodules are referenced to a master repository via commit
ID's, rather than branches. As such, changes to submodules need to be
committed to the master repo as well. That is, if you commit a change to
a submodule, the latest commit for that repository has progressed, and
that new status needs to be reflected in the master repo. If you make
changes to the submodule outside of the main repo working directory,
you'll need to git pull that submodule into the main working dir and
commit it's new pointer to that main repo.

When you deploy to WPE, for any submodule that the master repository
knows about, the commit for that submodule must be accessible to WPE.
This means 2 things: 1) The repo must be publicly readable. WPE does not
provide a public key for you to grant access to private repos. 2) The
commit to retrieve must be available on that remote (pushed)

The latter is often forgotten. Use the following one liner to push all
submodules from your master repo root dir::

    $ git submodule foreach git push master origin

It's typically easier to work on all files (main repo and submodules)
within the same working directory so that you can commit to both the sub
and main repos quickly.

If other users make changes to the sub-modules, you'll need to retrieve
these to your own local dir also. So before making any changes, it's
good practice to do the following::

    $ git pull origin master
    $ git submodule foreach git pull origin master

Contact
=======

-  For **non-technical issues** with the OKFN blog farm, e.g. with the
   content of one of its sites, contact either the owner of a specific
   blog site, or <blog@okfn.org>
-  For **technical problems** with the OKFN blog farm, notify either the owner
   of a specific blog site, or <sysadmin@okfn.org>.

There is also a mailing list <sysadmin-coord@lists.okfn.rg> for general
technical discussions.

Tickets (general)
-----------------

-  To raise a ticket in RT tracker, simply send a mail to <sysadmin@okfn.org>
-  If our blogfarm hoster WPEngine has to look into an issue, we raise a
   ticket in their support platform (see below)
-  There is `Issue tracker for http://okfn.org/
  <https://github.com/okfn/foundation/issues>`_ which tracks non-sysadmin
  issues with okfn.org, powered by Django CMS.
-  There is `Issue tracker for OKFN
  sites`<https://github.com/okfn/okfn.org/issues>`_, which tracks non-sysadmin
  issues on other OKFN sites.
-  Themes and plugins might have their own repo trackers, e.g. the
   "`Wordpress OKFN general-purpose theme (v2). Based on Bootstrap and
   Buddypress <https://github.com/okfn/wordpress-theme-okfn/issues>`__\ "

Tickets at WPEngine
-------------------

How to file a ticket at WPEngine:

-  If you haven't done so yet, register at `WPEngine's Zendesk
   platform <https://wpengine.zendesk.com/registration>`__. You have to
   use your @okfn.org mail address.
-  Log into `WPEngine's Zendesk
   platform <https://wpengine.zendesk.com/>`__. You should see
   "OKFN.org" organisation.
-  Click on "*SUBMIT A REQUEST*\ ".

Their core support times are 9:00-18:00 US Central Time (WPE sits in
Austin, Texas. CST=UTC-6, CDT=UTC-5). That is usually 15:00-24:00 UK
time. About out-of-hours support, `they
write <https://wpengine.zendesk.com/requests/125941>`__:

    *After hours Support and phone support focus primarily on emergency
    and high-priority issues. But we are getting closer and closer to
    being able to offer 24/7 Support. There are multiple team members
    here, answering phones and cleaning up tickets at virtually all
    hours of the day, every day. For now you can be confident that, when
    you have a problem, there will be someone here to help.*

Notes:

-  We are currently setup as a ZenDesk "shared organization" (as opposed
   to "non-shared"): everyone who registers with a @okfn.org address can
   access all our tickets.
-  There is a "*Subscribe*\ " button at "*OKFN.org*\ " ==> "*Open
   requests*\ "

How to report failing blog pages
--------------------------------

If blog pages fail please report it to us! But in order to look into the
failure, we need some details. Please collect the following data and
send it to <**sysadmin**\ @\ **okfn**.\ **org**>:

-  The **failing URL**. If the issue affects several URLs or sites
   please mention a couple of them (at least three).
-  The exact **time and date** of the failure, with timezone.
-  Your **IP address**. Click `here <http://the-i.de/>`__ to find out.
-  What is the **false result** (as opposed to the expected result)?
   E.g. copy&paste the error message.
-  (If you have a login to the site): Does it make a difference whether
   you are logged in or not?
-  Make sure you are logged out. Does the error disappear when you
   circumvent WPEngine's cache? (Append a random querystring like
   "?q=5971" to the URL)
-  (If yes, and you are a site admin) Does it help when you clear the
   WPEngine cache? (see section "Caching" below)

Don't bother to report failing blog pages if you don't have the time to
provide those details - there is nothing we can do without them.

Caching
=======

One can check whether a page was delivered from a cache by looking at
the headers (e.g. using ``curl -I`` or the Firefox Add-on `Live HTTP
Headers <https://addons.mozilla.org/en-US/firefox/addon/live-http-headers/>`__).
Example of a cached page:

| ``$ curl -sI ``\ ```http://okfn.org/`` <http://okfn.org/>`__\ `` | grep ^X-Cache``
| ``X-Cacheable: SHORT``
| ``X-Cache: HIT: 3``
| ``X-Cache-Group: normal``

One can view any page bypassing the cache by appending a unique (e.g.
random) query string. E.g.

| ``$ curl -sI ``\ ```http://okfn.org/?nocache=00012`` <http://okfn.org/?nocache=00012>`__\ `` | grep ^X-Cache``
| ``X-Cacheable: SHORT``
| ``X-Cache: MISS``
| ``X-Cache-Group: normal``

WPEngine uses Varnish, which caches aggressively. If this cache needs to be
busted, it needs to be ticketed with them.

How to: create a new blog...
============================

... as {name}.okfn.org
----------------------

Requirements:

-  You will need to be a Network Admin on okfn.org
-  You will need control over the DNS records for *okfn.org*
-  You will need access to the `WPEngine control
   panel <https://my.wpengine.com/>`__ (see below).

Basic install:

#. Login and go to Network Admin - http://okblogfarm.org/wp-admin/network/
#. Select Add Site

   -  For WG sites name after working group e.g. for economics wg would
      be economics.okblogfarm.org
   -  Put your username/email for admin role
   -  Test `http://{name}.okblogfarm.org/`, it should work now.

#. Add users to site as appropriate
#. Leave the "Network Admin" area. Instead, go to the admin area of you
   new blog at
   `http://{name}.okblogfarm.org/wp-admin/`
#. Activate and configure standard plugins:

   -  `Akismet <http://akismet.com/>`__
   -  Google Analytics (see Google Analytics in Settings)

#. Go go the `domain admin page
   <http://okblogfarm.org/wp-admin/network/settings.php?page=dm_domains_admin>`__.
   Add the site ID of your new site and the domain name if it needs to be
   `http://{name}.okfn.org/`, tick the `Primary` checkbox and submit the form.
#. Log into the `WPEngine control panel <https://my.wpengine.com/>`__
   then, add the new site hostname under
   `Domains <https://my.wpengine.com/installs/okf/domains>`__ (you might want
   to add redirects from www - optional)
#. Create a DNS CNAME record for `{{name}}.okfn.org` pointing to
   *blogfarm.okserver.org*. Wait for the record to propagate and test.
#. (Optional) Configure theme. The default Open Knowledge Foundation theme is maintained at
   https://github.com/okfn/wordpress-theme-okfn.
#. (Optional) Activate & configure additional plugins. Do this on a
   site-by-site basis, do **not** use 'Network Activate'

Remark:

-  Commonly used files should be hosted on Amazon S3 bucket
   http://assets.okfn.org. The process for uploading is documented at
   https://bitbucket.org/okfn/m.okfn.org/src/d7625d7066d0/m.okfn.org/README.txt

... as mydomain.org
-------------------

-  You will need control over the DNS records for *mydomain.org* (see
   `Sysadmin/DomainServices <Sysadmin/DomainServices>`__)
-  You will need access to the `WPEngine control
   panel <https://my.wpengine.com/>`__ (see below).
-  You will need to be a Network Admin on okfn.org.

#. Setup a new site as {name}.okfn.org as described in the previous
   paragraph.
#. Log into the `WPEngine control panel <https://my.wpengine.com/>`__
   then, add the new site hostname under
   `Domain <https://my.wpengine.com/installs/okf/domains>`__ (you might want
   to add redirects from www - optional)
#. Temporarily add the blog farm's IP address "*178.79.130.212
   mydomain.org*\ " to your /etc/hosts and test http://mydomain.org/.
#. Create a DNS CNAME record (see
   `Sysadmin/DomainServices <Sysadmin/DomainServices>`__) for
   mydomain.org (and www.mydomain.org) pointing to
   *blogfarm.okserver.org* or its IP address 178.79.130.212. If the
   domain is at DME, make it a "*A-NAME*\ " to *blogfarm.okserver.org*.
   Wait for the record to propagate and test.

How to: add or modify a theme/plugin
====================================

Caveats
-------

There are certain things to be aware of when manageing a wordpress
installation at WPEngine:

-  **Do not try to modify/update/upgrade the Wordpress core**. It is
   maintained by WPEngine
-  **Make minimal use of session cookies**. The presence of a session
   cookie might circumvent caches.
-  **Some PHP methods might be restricted or not available at all**.
   WPEngine might apply strict security policies and restrict what PHP
   can do. That could break plugins/themes.

WPE accounts
============

**TO BE DOCUMENTED**

How to: migrate an existing single-site WP instance into our blogfarm
=====================================================================

**TO BE DOCUMENTED**
