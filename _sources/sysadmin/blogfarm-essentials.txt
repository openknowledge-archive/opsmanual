This article contains guidelines for site owners how to activate polular
features on OKFN's Wordpress blogfarm at WPEngine. See
`Sysadmin/Blogfarm <Sysadmin/Blogfarm>`__ for general information about
the OKFN blogfarm.

Activate Akismet / Basic spam protection
========================================

Akismet is a comment and trackback spam prevention plugin that ships
with WordPress. Site administrators can implement basic spam protection
on their site by activating and configuring this plugin using the
following steps.

#. An API/Licence key is required for Akismet to function, you register
   for one at their website https://akismet.com/signup/
#. Log into the dashboard for your site (http://site.okfn.org/wp-admin)
#. Navigate to Plugins and activate Akismet
#. Navigate to Plugins > Akismet
#. Enter your API/Licence key then save

Disqus Comments
===============

As an alternative to the built-in WordPress comments feature, the Disqus
comments is a third party commenting system that may have more suitable
features and spam prevention methods. Site admins can enable Disqus
comments on their site using the following steps.

#. Signup at the Disqus website https://disqus.com/admin/signup/
#. Make note of the Site Shortname entry, and the login credentials of
   the Primary Moderator account you created
#. Log into your site dashboard (http://site.okfn.org/wp-admin)
#. Navigate to Plugins and activate the Disqus Comment System plugin
#. Navigate to Comments > Disqus
#. Login with the primary moderator account you created in the Disqus
   signup
#. Select the correct website profile and save
#. Disqus comments should now be active, verify this on your site. The
   Comments > Disqus menu option should now provide a portal to
   moderate/manage comments and settings

Google Analytics (single site)
==============================

Google Analytics tracks visitors of your site. Site administrators can
enable this on their site using the following steps/.

#. A Google Analytics tracking code is required, Google has existing
   help resources explaining how to do that -
   https://support.google.com/analytics/answer/1008015?hl=en
#. Log into your admin dashboard
#. Navigate to Settings > Google Analytics
#. Enter your tracking ID, then save.

NOTE: only available if you have rights to demonstrate you are the owner
of the site. (ftp access, template edition rights, etc)

Google Analytics (global tracking filter)
=========================================

Each site is allowed a unique Google Analytics tracking code as per the
above Google Analytics (single site). On top of this, a global OKF
tracking code (UA-33874954) is added to every page to log data aggregate
data across the entire blogfarm. No additional configuration is required
to include traffic for a new site in this aggregate account, however it
is possible to create a filter on this aggregate account to allow drill
down reports of traffic for each site that is tracked. Administrators of
the global tracking code may add this filter using the following steps.

#. Login to Google Analytics with a google account that has
   administrator access to the global profile
#. From the home screen of Google Analytics, select the admin button
   near the top right of the screen. (if when you log in you do not land
   on the home screen, you should click the home button on the top left.
   These navigation instructions get quite confusing if starting from a
   different page).
#. You should now be on the Account Administration screen with a table
   listing of the accounts your login has access to. Within this table
   click on "OKFN 2".
#. Then select "OKFN Aggregate" from the Properties table on the next
   screen.
#. Here you should see a list of "Profiles" within this property. Do not
   edit the existing profiles, simply select "New Profile"
#. Leave "Web Site" selected under the "what data should this profile
   collect" question.
#. For the "Reporting Profile Name", enter the name of the site
#. Select a relevant reporting timezone
#. Click "Create Profile"
#. On the resulting new profile screen, select the "Filters" tab.
#. Click "New Filter"
#. Select "Apply Existing Filter"
#. In the "Available Filters" list, scroll down and select the first
   "Add Hostname" item and click "Add" to add it to the "Selected
   Filters" list
#. Click "Save"
#. Return to the Filters List (by clicking save if you have landed on
   the "Edit Filter" page.
#. Click "New Filter" again
#. This time leave the "Create New Filter" option selected
#. In the filter name, insert "Include Domain (example.com)" where the
   item in parenthesis is the url of the site being filtered. This is
   just for identification purposes.
#. Select the "Custom Filter" option
#. Then select the "Include" option
#. Then from the "Filter Field" dropdown select "Hostname"
#. Then in the filter pattern enter the domain, escaping periods with
   backslashes (eg publicdomainreview\\.org or blog\\.okfn\\.org)
#. Leave case sensitive as "No"
#. Click "Save"
#. It may take some hours for traffic stats to appear for this profile
   in the reporting section
