Google Analytics
################

Adding a New Site to Google Analytics
=====================================
When there is a request to create a new Google Analytics tracking ID, it can be
done with the following steps.

#. Login to Google Analytics with a google account that has administrator
   access.
#. From the home screen of Google Analytics, select the admin button
   near the top right of the screen. (if when you log in you do not land
   on the home screen, you should click the home button on the top left.
   These navigation instructions get quite confusing if starting from a
   different page).
#. You should now be on the Account Administration screen with a table
   listing of the accounts your login has access to. Within this table
   click on "OKFN 2".
#. You should now be on the Account Administration screen with a table
   listing of the accounts your login has access to. Within this table
   click on "Create a new property". This should be at the bottom of the list.
#. Enter the website name (e.g. Open Development Tookit) and website URL
   (e.g. http://opendevelopmenttoolkit.net). Change the Reporting Time Zone to
   United Kingdom and select "GMT (no daylight saving)".
#. Click on "Get Tracking ID". This will get you to a page where the tracking
   ID will be promimently displayed.
#. Go back to the Admin page and select the new Property. The view on the third
   column will be called "All Website Data". Click on "View Settings" and
   rename that to the property name.


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
   backslashes (eg ``publicdomainreview\.org`` or ``blog\.okfn\.org``)
#. Leave case sensitive as "No"
#. Click "Save"
#. It may take some hours for traffic stats to appear for this profile
   in the reporting section
