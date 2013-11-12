\ **DISCLAIMER**

**THIS ARTICLE CONTAINS OUTDATED HISTORIC INFORMATION! Our blogfarm is
no longer hosted at Page.ly. The content of this article was originally
in `Sysadmin/Blogfarm <Sysadmin/Blogfarm>`__ and is only kept here for
archeological purposes.**\

----

OKF has a blog farm at specialized Wordpress hoster
`Pagely <https://page.ly/>`__. It is a wordpress multisite installation
that serves numerous properties from a single codebase. It can provide
sites at **{name}.okfn.org** as well as at any domain name (via
Wordpress' domain alias feature)..

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
<`**blog-coord**\ @\ **lists**.\ **okfn**.\ **org** <http://lists.okfn.org/mailman/listinfo/blog-coord>`__\ >
for general technical discussions.

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
   circumvent Pagely's cache? (Append a random querystring like
   "?q=5971" to the URL)
-  (If yes, and you are a site admin) Does it help when you clear
   Pagely's cache? (see section "Caching" below)

Don't bother to report failing blog pages if you don't have the time to
provide those details - there i nothing we can do without them.

Tickets (general)
-----------------

-  Single fails are logged in our Google Speadsheet *blogfarm issue
   reports*, see previous section *How to report failing blog pages*

-  Sysadmin issues with the wordpress blogfarm are traced in our
   [http://trac.okfn.org/query?status\ =!closed&component=blogfarm trac]
   in the trac component "blogfarm".
-  If Pagely has to look into an issue, we raise a ticket in their
   support platform (see below)
-  There is also "`Issue tracker for http://okfn.org/ and other OKFN
   websites <https://github.com/okfn/okfn.org/issues>`__\ " which tracks
   non-sysadmin issues e.g. with layout and design.
-  Themes and plugins might have their own repo trackers, e.g. the
   "`Wordpress OKFN general-purpose theme (v2). Based on Bootstrap and
   Buddypress <https://github.com/okfn/wordpress-theme-okfn/issues>`__\ "

Tickets at Pagely
-----------------

Annoyingly, at Pagely the Ticket system (pagely.zendesk.com) username
has to be identical to the mail contact address at the control panel
(atomic.pagely.com). Since our blog sysadmin Bobby need to see
notifications, we had to introduce a new alias *blog-sysadmin@okfn.org*.
Therefore:

-  **To *raise* a new ticket: log into the `Pagely control
   panel <https://atomic.pagely.com/>`__ as *sysadmin@okfn.org* and go
   to "Support". Do not forget to sign it with your name.**
-  **To *view* or *comment* an existing ticket: log into the `Pagely
   support platform <https://pagely.zendesk.com/>`__ as
   *blog-sysadmin@okfn.org* and go to "Check your existing requests." Do
   not forget to sign your comments with your name.**

See section *Pagely accounts* below for more details.

Known issues
============

There is a set of known issues with the blog farm at pagely. Some of
them should be solved by now. Here is an unsorted history. (Note that
older Pagely tickets have to be viewed not with the current
'blog-sysadmin@' ticket account, but with our former 'sysadmin@'
account)

-  Failing blog pages:

   -  (**Please report failing pages according to above section *How to
      report failing blog pages*!**)
   -  (last report 2012-10-31) Intermittant bogus redirects, e.g.
      http://okfn.org/support ==>
      http://publicdomainreview.org/support/. They disappear after a few
      minutes.
   -  (during 2012-11-02, no report since) During a whole day an error
      "*This webpage is not available. The connection to okfn.org was
      interrupted.*\ " on several blogs.
   -  (last report pre 2012-10) Bogus warning page "*What the heck? Our
      security droid has flagged this request.*\ " instead of the
      (legitimate) page. We suspect that Pagely's security system
      sometimes conceils php error pages this way. We might find out
      once we have access to the logs (see below).
   -  (last report 2012-10-??) [STRIKEOUT:"*Page Not Found*\ " on
      http://okfn.org and http://blog.okfn.org, not able to login]. Was
      an outage of Pagely's hoster "Firehost".
   -  [STRIKEOUT:Outdated pages] should be fixed, see below.
   -  [STRIKEOUT:403 pages] should be fixed, see below.

-  Buddypress password change form produces wrongly encoded URL
   `ticket <https://support.pagely.com/requests/16307>`__.
-  We currently do not have a good spam-prevention plugin, see
   `ticket <http://trac.okfn.org/ticket/1320>`__. Both previous plugins
   (*"SI-captcha"* and *"Bad behaviour"*) turned out to be incompatible
   with Pagely. We will probably try Disqus, at least for comments.
-  We currently do not have access to log files yet. That makes
   debugging hard. Pagely is working on it (`ticket
   1 <https://support.pagely.com/requests/15766>`__, `ticket
   2 <https://support.pagely.com/requests/16490>`__)
-  We currently have no metrics besides our own Google Analytics stats.
   What we would need is (A) Munin-style ingres/egress traffic stats and
   (B) Pingdom-style response-time monitoring. We might be able to
   generate our own traffic stats once we have access to he logs.
-  Pagely `complained <https://support.pagely.com/requests/16501>`__
   (https://support.pagely.com/requests/15239 again]) that our blogfarm
   would use "too much traffic" and asked whether we wanted either to
   pay additional traffic fees, or to subscribe to a CDN service.
-  Pagely's Zendesk ticket system does not allow to CC easily (see
   section "*tickets*\ " above)
-  Pagely likes to close tickets very quickly, sometimes prematurely.
   Once they closed a ticket, we can neither re-open nor comment. Only
   option is to file a follow-up ticket.
-  Pagely sometimes makes unlikely claims concerning the cause of a
   problem without providing technical detail or evidence. E.g. when
   many blogs suddenly failed they claimed that the Domain Mapping
   Plugin was misbehaving but didn't say what was wrong or how they
   fixed it (`ticket <https://support.pagely.com/requests/15471>`__).
-  Our "Striking" theme
   `disables <https://support.pagely.com/requests/15660>`__ posting
   sanitation. We need to `check <http://trac.okfn.org/ticket/1321>`__
   whether that is a security issue.
-  The presence of session cookies circumvents the Pagely caches and
   should be avoided for non-logged-in users (except maybe
   login/registration pages).
-  Some PHP methods might be restricted or not available at all. Pagely
   applies strict security policies and restricts what PHP can do. That
   sometimes breaks plugins/themes (e.g. a DB-backup plugin).
-  We currently do not have daily DB dumps. Bobby and Joel are `working
   on it <http://trac.okfn.org/ticket/1310>`__. (Note that Pagely
   `refused <https://support.pagely.com/requests/15390>`__ to assist
   when a backup plugin could not write to disk, even though they once
   `promised <https://support.pagely.com/requests/14272>`__ to provide
   daily DB backup).
-  We do not yet have a staging blog server. (Note that we do not have
   access to the Wordpress core Pagely is using)
-  We have a git repository on s070, but it only covers the core OKFN
   themes. Bobby is working on version-controlling the rest of
   wp-content/{themes,plugins,mu-plugins,...}/ . This is not
   straghtforward because we only have very slow FTPS access
   (`ticket <http://trac.okfn.org/ticket/1325>`__).
-  We currently have theme/plugin-upload-via-webinterface enabled again
   (`ticket <http://trac.okfn.org/ticket/1325>`__). We should either
   disable it or, or at least shrink the set of superadmins drastically
   (`ticket <http://trac.okfn.org/ticket/911>`__).

Solved issues
-------------

-  Outdated pages. We experienced them a lot at the beginning. Clearing
   the Pagely caches usually helped.
-  403 pages. `Fixed <http://trac.okfn.org/ticket/1313>`__. They were
   caused by a plugin not compatible with Pagely (*"Bad behaviour"*) in
   combination with error page caching (see below).
-  Occasionally/intermittently produced error pages were cached at
   Pagely and served for hours.
   `Fixed <http://trac.okfn.org/ticket/1314>`__.
-  Slow response times on many blogs. There were occasional complaints
   about the blogfarm being slow. We think that was caused by the (now
   removed) SI-Captcha plugin setting session cookies on every page of
   the heaviest blog site PDR, thus circumventing the Pagely caches and
   slowing down the whole Wordpress instance.
-  Pagely used to not send mail notifications about scheduled downtimes
   but to only post about (twitter, dashboard, their blog). They
   promised to send mail notifications next time
   (`ticket <https://support.pagely.com/requests/15671>`__)

Caching
=======

Pagely applies heavy caching on our blogfarm: every blog site has
automatically activated the `Super
Cache <http://wordpress.org/extend/plugins/wp-super-cache/WP>`__ plugin.
And the whole blog farm is cached by
`Varnish <https://www.varnish-cache.org/>`__.

The caches speed up our blog farm significantly, but they also come with
some downsides:

-  The presence of a session cookie will circumvent the Varnish cache.
   Therefore, blog sites (aka their theme and their plugins) should make
   minimal use of sessions cookies. If possible, only use them on e.g.
   login/register/password-reminder pages.
-  Sometimes the cache delivers outdated pages. It can help to purge the
   caches, see below.

One can check whether a page was delivered from the Varnish cache by
looking at the headers (e.g. using ``curl -I`` or the Firefox Add-on
`Live HTTP
Headers <https://addons.mozilla.org/en-US/firefox/addon/live-http-headers/>`__).
If the headers ``X-Cache: V2HIT`` and ``Age`` are both >0, then the page
has been delivered from cache. Example of a cached page:

| ``$ curl -sI ``\ ```http://okfn.org/`` <http://okfn.org/>`__\ `` | egrep '^(X-Cache|Age):'``
| ``X-Cache: V2HIT 28``
| ``Age: 2264``

One can view any page bypassing the cache by appending a unique (e.g.
random) query string. E.g.

| ``$ curl -sI ``\ ```http://okfn.org/?nocache=00012`` <http://okfn.org/?nocache=00012>`__\ `` | egrep '^(X-Cache|Age):'``
| ``X-Cache: V2MISS``
| ``Age: 0``

Recommended way to purge the caches at Pagely
---------------------------------------------

There are **many** (!) places where one can purge the caches. The steps
detailed here are the suggested method of clearing the cache as it's
both the simplest and seemingly most effective solution for dealing with
a site/page specific issue. Alternative methods and their drawbacks are
noted in the next section.

#. In your main browser, log into your site admin panel at
   ${blogsite}/wp-admin/
#. Look for the "Hosting Panel" menu option on the left hand side and
   click on the "Purge Cache" sub menu here. The page should reload with
   a message of indicating that both Super Cache purge and Varnish cage
   purge were performed successfully.
#. Check to see if the issue has been resolved. Make sure that you are
   **not logged into the site**, and that your browser **has its browser
   cache cleared!** We recommend you use

   #. either an incognito browser window (Firefox: *Tools* ==> *Start
      Private Browsing*);
   #. or an alternative browser;
   #. or, if you only want to check the HTTP return code, use a command
      line tool like ``curl -I``.

#. When you have to repeat the check, make sure your browser's cache is
   clean again!

   #. If you use an incognito browser window, close and re-open it
   #. If you use an alternative browser: clear its cache (Firefox:
      *Tools* ==> *Clear Recent History* ==> make sure *Cache* is ticked
      ==> *OK*). Or if the browser supports it do a
      "Force-reload-without-cache" (Firefox: Hold when clicking the
      reload button; or press ++)
   #. If you use a commend like tool like curl, just repeat it. They
      usually don't cache and don't hold session cookies.

Checking without being logged in is important as both Super Cache and
Varnish have methods of bypassing cached content when viewing pages as
logged in user, making it seem like a cache issue is resolved when in
fact it is not. We recommend to use the first browser to perform
administrative actions, and the second to check the result from the user
perspective.

Checking with a clear cache is important because often the server side
cache issue has already been resolved but your browser will continue to
show the incorrect result as retrieved from its local cache.

Alternative methods to purge the caches at Pagely
-------------------------------------------------

So far we found six ways to purge the two caches. Unfortunately Pagely
does not provide documentation of how they work and interact. It is
currently not 100% clear how the different methods relate but here is a
best guess description of each, derived from our experiments:

-  As a blog site admin, you can purge the caches for your specific
   ${blogsite}. Log into your blog site admin panel at
   ${blogsite}/wp-admin/

   -  [Method 1] In the left navigation bar, go to *Hosting Panel* and
      press ''"**Purge Cache**\ "'

      -  This is the preferred method. It removes all the site's pages
         from WPSC **and** from Varnish.

   -  [Method 2] In the dashboard's top navigation bar, click on
      *"**Delete cache** (Delete cache of the current page)"*

      -  This removes all the site's pages from WPSC
      -  Clearing pages from WPSC only has turned out to not be very
         effective because pages are likely to be also in the Varnish
         cache. It should be followed by a Varnish purge.

   -  [Method 3] In the left navigation bar, go to *Settings* ==> *WP
      Super Cache*, then to tab *Easy* and press the button *"**Delete
      Cache >>**\ "*

      -  This removes all the site's pages from WPSC. Same objection
         applies as in method 2.

   -  [Method 4] At the same place, there is also a button *"**Delete
      Cache On All Blogs**\ "*

      -  This probably removes all pages *of all sites* from WPSC! Same
         objection applies as in method 2.

   -  [Method 5] At the same place above the *Easy* tab, click on
      *"**Click to purge the Varnish cache.**\ "*

      -  This removes all the site's pages from Varnish.
      -  As a follow up to purging WPSC (Methods 2/3) this is usually
         effective but currently doesn't appear to work 100% of the
         time.

-  As a OKFN sysadmin, you can purge Varnish across sites. Log into the
   `Pagely control panel <https://atomic.pagely.com/>`__

   -  [Method 6] Go to *Manage ... okfn.org* ==> tab *Actions* and press
      *"**Purge cache**\ "*

      -  This removes all pages *of all sites* from Varnish.
      -  Combined with purging all sites from WPSC (method 4), this is a
         *purge everything everywhere*. It is typically quite effective.
      -  Note that upon completion, Varnish return codes of the purging
         operation are displayed on the control page. This suggests that
         the function is simply looping through all sites on the
         blogfarm and requesting the Varnish purge url for each, then
         returning the result. Currently a mixture of 404 and 200 codes
         are returned suggesting that this is not working for all sites.

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
-  You will need access to the `Pagely control
   panel <https://atomic.pagely.com/>`__ (see below).
-  You will need to be a Network Admin on okfn.org.
-  If your want the blog to be cached, you will need superuser access to
   our squid cache, or ask someone who has.

#. Setup a new site as {name}.okfn.org as described in the previous
   paragraph.
#. Log into the new site's admin dashboard (/wp-admin/) and go to
   *Tools* ==> *Domain Mapping* (if there are two entries called *Domain
   Mapping*, use the top one) and enter the additional domain name(s)
   (www.)mydomain.org. Leave {name}.okfn.org the default domain for now.
#. Log into `Pagely control panel <https://atomic.pagely.com/>`__, go to
   *My Sites* ==> *Manage ... okfn.org*, then tab *Domain aliases* ==>
   *Add Alias* and enter mydomain.org (repeat for www.mydomain.org). Log
   out.
#. Temporarily add the blog farm's IP address "*193.34.146.141
   mydomain.org*\ " to your /etc/hosts and test http://mydomain.org/.
#. Create a DNS record (see
   `Sysadmin/DomainServices <Sysadmin/DomainServices>`__) for
   mydomain.org (and www.mydomain.org) pointing to
   *blogfarm.okserver.org* or its IP address 193.34.146.141. If the
   domain is at DME, make it a "*A-NAME*\ " to *blogfarm.okserver.org*.
   Wait for the record to propagate and test.
#. If all is working as expected, go back to the admin dashboard of your
   site *Tools* ==> *Domain Mapping* and make (www.)mydomain.org the
   default domain.

Note:

-  See this tutorial (start at step 4 -- plugin is already installed for
   you!):
   http://ottopress.com/2010/wordpress-3-0-multisite-domain-mapping-tutorial/

How to: add or modify a theme/plugin
====================================

Caveats
-------

There are certain things to be aware of when manageing a wordpress
installation at Pagely:

-  **Do not try to modify/update/upgrade the Wordpress core**. It is
   maintained by Pagely
-  **Make minimal use of session cookies**. The presence of a session
   cookie will circumvent the Varnish cache. E.g. we had to disable the
   SI-capture plugin on the comment section of postings. If possible,
   only use them on e.g. login/register/password-reminder pages.
-  **Some PHP methods might be restricted or not available at all**.
   Pagely applies strict security policies and restricts what PHP can
   do. That sometimes breaks plugins/themes. E.g. when Bobby wrote a
   plugin that would dump the DB to disk, it could pull the DB, but then
   it would not write the dump file to disk. (To be elaborated).

Modifying the Main Themes
-------------------------

**You will need:** SSH access to ``okfn@s070.okserver.org``, and a
working knowledge of Git.

The themes ``okfestival-wordpress`` and ``wordpress-theme-okfn`` are
stored in a git repository on the server.

::

    git clone okfn@s070.okserver.org:git/wordpress-theme-okfn

::

    git clone okfn@s070.okserver.org:git/okfestival-wordpress

You can then push & pull changes as required. Note that every time you
push to the server there follows a very slow post-receive hook which
copies your updated files into Page.ly.

Modifying Other Themes & Plugins
--------------------------------

**You will need:** SSH access to ``okfn@s070.okserver.org``, and a
working knowledge of Bash and SSH.

You can see installed plugins here:

::

    ssh okfn@s070.okserver.org
    ls -l pagely-mount/wp-content/plugins

Use ``scp`` to add your plugin to that folder, or to add a theme to
``pagely-mount/wp-content/themes``.

Note that this is extremely slow. ``pagely-mount/`` is an FTP
filesystem; every filesystem operation interacts with the Pagely servers
via FTP.

If you're going to do a lot of active development of a theme or plugin,
consider setting up a git repository like the ones above. Use the
existing examples as a starting point, with modifications to
``hooks/post_receive``.

Git/Phing Deployment Procedure
==============================

Intro
-----

Read more about Phing at the project homepage
(http://www.phing.info/trac/).

The procedure outlined below has been designed with a very basic
methodology in mind, untested code should never be pushed directly to a
production environment. The steps below outline a basic system that
should allow authorised users to efficiently and safely contribute code.

#. **Build Locally**: Every person responsible for contributing code
   should have a completely operational local environment that matches
   the production environment as closely as possible. All modifications
   are made locally.
#. **Test, then commit to the staging branch**: Test your modifications
   in your local environment whenever you reach a milestone. Verify that
   the changes you're about to commit to VCS will not adversely effect
   other developers working on the code base.
#. **Deploy to staging, test again**: Send your changes to the staging
   server, test them again. Don't assume that just because it works on
   your local environment, it must work on the staging server. It has
   been designed to mirror the production site for the purpose of being
   a test bed, use it.
#. **Repeat the steps above for any issues encountered**
#. **Flag your commit as production ready**: Once you're satisfied that
   your changes are suitable for production, flag them as ready to me
   merged into the master branch (to whom should they be flagged?). You
   may need to provide all commit ID's associated with your changes for
   cherry picking.
#. **Test on production**: Once your changes have been deployed to the
   main branch and deployed onto the production site, test them again.

Merging and deployment of the main branch to the production environment
should be performed on a consistent schedule. This philosophy has a few
benefits.

#. To ensure that there has been adequate time for the automated
   monitoring systems on the staging environment to detect any behind
   the scenes issues
#. To allow contributors and site owners to plan and coordinate their
   modifications
#. Ensure that there are adequate sysadmin resources on call to deal
   with any emergency issues

(Need to discuss a schedule that is suitable for all parties here)

Setting Up a Local Dev Environment
----------------------------------

#. Install Apache2, PHP, MySQL and Pear (typically best from maintainer
   packages rather than bundled packages such as MAMP/XAMPP)
#. Install a command line ssh/scp/rsync client. Consider Cygwin for
   Windows (http://www.cygwin.com/), Macports for mac
   (http://www.macports.org/).
#. Install a command line git client. Consider msysgit for Windows
   (http://code.google.com/p/msysgit/).
#. Ensure that the following commands are in your PATH environment
   variable so that they may be called from any location.
   ssh/scp/rsync/git/mysql/mysqldump
#. Add the following minimum entries to your hosts file. Add additional
   items for any sub-domain you're working on.

::

    127.0.0.1 okfn.dev www.okfn.dev dev.okfn.dev

#. Configure a local Apache2 virtual host. A very basic example:

::

    <VirtualHost *:80>
        ServerName okfn.dev
        ServerAlias *.okfn.dev
        DocumentRoot "/var/www/okfn/okfn.org"
        <Directory "/var/www/okfn/okfn.org">
            AllowOverride All
        </Directory>
    </VirtualHost>

N.B. The html directory is contained within the root directory of the
git repo.

#. Make sure Pear is up-to-date and configured to use the alpha channel

::

    pear config-set preferred_state alpha
    pear channel-update pear.php.net
    pear upgrade

#. Install Phing into Pear

::

    pear channel-discover pear.phing.info
    pear channel-discover pear.phpunit.de
    pear channel-discover pear.pdepend.org
    pear channel-discover pear.phpmd.org
    pear channel-discover pear.docblox-project.org
    pear install --alldeps phing/phing

#. Clone the git repo to your local environment

::

    cd /var/www/okfn/
    git clone git@git.okserver.org:wordpress ./

#. Create 2 local mysql databases, (main branch: "okfn"/ dev branch
   "okfn\_dev"). Grant access to a user of your choice to both
   databases.
#. Copy "build.conf/wp-config-local-dev.php.sample" to
   "build.conf/wp-config-local-dev.php" and modify the contents to suit
   your setup. (Attach to dev db)
#. Copy "build.conf/wp-config-local-master.php.sample" to
   "build.conf/wp-config-local-master.php" and modify the contents to
   suit your setup (Attach to main db)
#. Copy "build.conf/local.properties.sample" to
   "build.conf/local.properties" and modify the contents to suit your
   setup
#. Run the Phing initialization task to setup your local environment.
   This will clone the production site, including database and user
   uploaded files.

Things to do before you start working
-------------------------------------

#. Ensure you have retrieved the most recent changes from the git server

::

    cd /var/www/okfn
    git pull

#. Run one of the Phing redownload tasks to sync the appropriate db and
   user uploaded files to your local environment

::

    phing redownload
    phing redownload-db
    phing redownload-dev
    phing redownload-dev-db

#. Checkout or create a new local branch unique to you

::

    git checkout bobby

#. If you need to reset your local db to the last download, run:

::

    phing reset
    phing reset-dev

To deploy your changes
----------------------

#. Ensure your tested changes have been committed to your unique branch
#. Switch to the "dev" branch and merge in your unique branch

::

    git checkout dev
    git merge bobby
    git commit -am 'Merging tested feature'

#. Run Phing deploy-dev task to push and deploy changes to the staging
   server.

::

    phing deploy-dev

Important Notes
---------------

#. It's suggested that the number of characters in the domain used for
   your local environment match the number of characters for the primary
   domain. Eg okfn.org > okfn.dev. Read the link below for a detailed
   description on why. Essentially it's to ensure that when the Phing
   script runs a search & replace on the database you download we don't
   break any serialised arrays. Important to note that the staging site
   uses a similar script but does not match the character length, thus
   you'll find missing content items (like widgets) on the staging site.
   (Yet to find a workaround to this)
   http://stackoverflow.com/a/8060418/324441
#. Avoid using serialised arrays that may contain full urls in your
   plugins or themes. Where possible, use relative links when populating
   content.
#. Domain redirects are disabled on your local environment and on the
   staging server. That is, publicdomainreview.okfn.dev not
   publicdomainreview.dev (This item is incomplete)

Pagely accounts
===============

-  The main control panel

   -  Raise new tickets here. **Make sure you sign comments with your
      name!**
   -  URL: https://atomic.pagely.com/
   -  Primary Domain: okfn.org
   -  Username: sysadmin@okfn.org (this is only the *username* here. The
      actual contact mail address is set to *blog-sysadmin@okfn.org*)
   -  Password: Ask `Sysadmin <Sysadmin>`__ or see GDoc *Wordpress
      Sysadmin Info*

-  The ticket system

   -  Viewing and comment on existing tickets here. **Do no raise new
      tickets here! Make sure you sign comments with your name!**
   -  URL: https://pagely.zendesk.com/
   -  Username: blog-sysadmin@okfn.org (Use the old Zendesk username
      *sysadmin@okfn.org* to view old tickets).
   -  Password: Ask `Sysadmin <Sysadmin>`__ or see GDoc *Wordpress
      Sysadmin Info*

-  File access to the WP codebase:

   -  ftps://f3.pressftp.com (port 990)
   -  Username: ftp46NjjirT59KpzYg
   -  Password: Ask `Sysadmin <Sysadmin>`__ or see GDoc *Wordpress
      Sysadmin Info*

Note that ftps is NOT sftp and not very well supported. See e.g.
`#1268 <http://trac.okfn.org/ticket/1268#comment:9>`__.

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
