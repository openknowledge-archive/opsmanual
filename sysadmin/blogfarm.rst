**Note that we recently migrated to our new wordpress hoster
`WPEngine <http://wpengine.com>`__. Some parts of this article are not
yet updated and lack details about WPEngine. We will close those gaps
shortly.**

Editors, look out for the tag "*TO BE DOCUMENTED*\ "

--------------

OKF has a blog farm at specialized Wordpress hoster
`WPEngine <http://wpengine.com>`__. It is a wordpress multisite
installation that serves numerous properties from a single codebase. It
can provide sites at **{name}.okfn.org** as well as at any domain name
(via Wordpress' domain alias feature).

Amongst the sites it currently hosts are the `OKFN
website <http://okfn.org/>`__, the `OKFN
blog <http://blog.okfn.org/>`__, local OKFN chapters like `OKFN
Belgium <http://okfn.be/>`__ or `OKFN Germany <http://okfn.de/>`__, the
`Public Domain Review <http://publicdomainreview.org/>`__,
`CKAN <http://ckan.org/>`__, the `Open Knowledge & Data
Festival <http://okfestival.org/>`__,
`OpenGLAM <http://openglam.org/>`__, `Digitised Manuscripts to
Europeana <http://dm2e.eu/>`__ and many others.

See also
========

-  `Sysadmin/Blogfarm Essentials <Sysadmin/Blogfarm Essentials>`__
   contains guidelines for site owners how to activate polular features
   in this blogfarm.
-  `Sysadmin/Blogfarm Code
   Deployment <Sysadmin/Blogfarm Code Deployment>`__ describes how to
   deploy code to this blogfarm.
-  `Sysadmin/Blogfarm at Pagely <Sysadmin/Blogfarm at Pagely>`__ An
   outdated historic version of this article when it was hosted at
   Pagely

Contact
=======

-  For **non-technical issues** with the OKFN blog farm, e.g. with the
   content of one of its sites, contact either the owner of a specific
   blog site, or <**blog**\ @\ **okfn**.\ **org**>
-  For **technical problems** with the OKFN blog farm, notify either the
   owner of a specific blog site, or
   <**sysadmin**\ @\ **okfn**.\ **org**> (See also
   `Sysadmin <Sysadmin>`__).

Note that our current wordpress codebase maintainer is Bobby, who is not
on the sysadmin alias. The OKF core sysadmin will notify him where his
involvement is needed. There is a mail alias that includes him as well
(), but we only use it internally for alerts and ticket notifications.

There is also a mailing list
<`**sysadmin-coord**\ @\ **lists**.\ **okfn**.\ **org** <http://lists.okfn.org/mailman/listinfo/sysadmin-coord>`__\ >
for general technical discussions.

Tickets (general)
-----------------

-  Single fails are logged in our Google Speadsheet *blogfarm issue
   reports*, see section *How to report failing blog pages* below
-  To raise a ticket in OKFN's Tenderapp tracker, simply send a mail to
   <**sysadmin**\ @\ **okfn**.\ **org**> (See also
   `Sysadmin <Sysadmin>`__)
-  [STRIKEOUT:Sysadmin issues with the wordpress blogfarm are traced in
   our [http://trac.okfn.org/query?status\ =!closed&component=blogfarm
   trac] in the trac component "blogfarm".]
-  If our blogfarm hoster WPEngine has to look into an issue, we raise a
   ticket in their support platform (see below)
-  There is also "`Issue tracker for http://okfn.org/ and other OKFN
   websites <https://github.com/okfn/okfn.org/issues>`__\ " which tracks
   non-sysadmin issues e.g. with layout and design.
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
-  Click on "*SUBMIT A REQUEST*\ ". Bobby, Joel and Nils should be
   automatically CCed onto the ticket.

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
either enter it into our Google spreadsheet "*blogfarm issue report*\ ",
or send it to <**sysadmin**\ @\ **okfn**.\ **org**>:

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
provide those details - there i nothing we can do without them.

Caching
=======

**TO BE DOCUMENTED**: How does caching works at WPEngine, and how can we
manage the cache at WPEngine

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

How to: create a new blog...
============================

... as {name}.okfn.org
----------------------

Requirements:

-  You will need to be a Network Admin on okfn.org

Basic install:

#. Login and go to Network Admin - http://okfn.org/wp-admin/network/
#. Select Add Site

   -  For WG sites name after working group e.g. for economics wg would
      be economics.okfn.org
   -  Put your username/email for admin role
   -  Test `http://{name}.okfn.org/ <http://{name}.okfn.org/>`__, it
      should work now (There is a wildcard DNS CNAME \*.okfn.org ==>
      blogfarm.okserver.org)

#. Add users to site as appropriate
#. Leave the "Network Admin" area. Instead, go to the admin area of you
   new blog at
   `http://{name}.okfn.org/wp-admin/ <http://{name}.okfn.org/wp-admin/>`__
#. Activate and configure standard plugins:

   -  `Akismet <http://akismet.com/>`__
   -  Google Analytics (see Google Analytics in Settings)

#. (Optional) Configure theme. The default OKFN theme is maintained at
   https://bitbucket.org/okfn/wp-theme-okfn.
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
   `1 <https://my.wpengine.com/installs/okf/domains>`__ (you might want
   to add redirects from www - optional)
#. Temporarily add the blog farm's IP address "*178.79.131.171
   mydomain.org*\ " to your /etc/hosts and test http://mydomain.org/.
#. Create a DNS CNAME record (see
   `Sysadmin/DomainServices <Sysadmin/DomainServices>`__) for
   mydomain.org (and www.mydomain.org) pointing to
   *blogfarm.okserver.org* or its IP address 178.79.131.171. If the
   domain is at DME, make it a "*A-NAME*\ " to *blogfarm.okserver.org*.
   Wait for the record to propagate and test.

Note:

-  See this tutorial (start at step 4 -- plugin is already installed for
   you!):
   http://ottopress.com/2010/wordpress-3-0-multisite-domain-mapping-tutorial/

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

#. Ensure you have the access level you need:

   -  Admin access to the old blog
   -  Superadmin access to the blogfarm
   -  File system read access to the old blog's ${WP\_ROOT}/wp-content/
      . Make sure rsync+ssh are installed and working.
   -  File system write access to the blogfarm's ${WP\_ROOT}/wp-content/

#. [*To be cone by OKF core admin*\ ] Domain preparation:

   -  On s006.okserver.org=cache-euw1.okserver.org (soon
      s050.okserver.org=cache-sov.okserver.org, see #881), configure the
      domain in question into /etc/squid3/squid.conf. For now, point it
      to the old blog. Re-load cache and test with your local
      /etc/hosts.
   -  Configure the DNS record(s) of the domain in question to point to
      the very cache. When the new records have propagated, unconfigure
      the domain name from your /etc/hosts.

#. Notify blog users about the blog being read-only for the time of the
   migration.
#. Disable editing on the old blog
#. Export the old blog into a file
#. Temporarily apply Rufus' famous import-fix to the blogfarm's
   /home/okfn/var/wp/okfn.org/wp-includes/post.php (see #553). In line
   2436 (as of WP 3.2.1 - line number will shift with future releases),
   comment out this:
   ::

       // $postarr = sanitize_post($postarr, 'db');

#. Create an empty new blog in the blog farm
#. Import the blog-export-file into the new farm site. During this
   process, many users in the old blog will either not have an account
   in our blog farm, or have a different username (but same mail
   address). Map the usernames of
   existing-users-with-different-username, and create missing users.
#. rsync the old blog's ${WP\_ROOT}/wp-content/ to the blog farm.
#. Verify & test all went well (/etc/hosts trick). If not, delete the
   site in the blog farm and return to [7]
#. Revert the above 1-line patch
#. [*To be cone by OKF core admin*\ ] In the cache server's
   /etc/squid3/squid.conf, point the domain to *blogfarm.okserver.org*
#. Undo your /etc/hosts test line and check again.
#. Notify blog users that the blog is fully operational again. Warn them
   that those clients which use outdated DNS entries might still end up
   on the old read-only blog.
#. Once the DNS changes from [2] have propagated (usually after 1 day),
   disable the old blog.
