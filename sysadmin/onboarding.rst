Onboarding staff and managing leavers
=====================================

* Managers should email HR with details. For new joinees, it will involve
  sending an email with their full name and start date. For leavers, mention
  the name, email and final date. Also specify if emails need to be forwarded
  after the employee has left.
* HR will open a ticket with sysadmin about the new joinee. Sysadmins will
  create accounts and log actions to the ticket and close it out when
  everything is ready.

Joiners
-------



+-------------------------------------------------------+----------+--------+
| Action                                                | Team     | Req?   |
+=======================================================+==========+========+
| Create Google Apps account                            | sysadmin | Yes    |
+-------------------------------------------------------+----------+--------+
| Add to relevant Google Groups                         | sysadmin | Yes    |
+-------------------------------------------------------+----------+--------+
| Collect all new starter info, inc copy of Passport    | HR       | Yes    |
+-------------------------------------------------------+----------+--------+
| Add email alias to FastMail, and wait 15m for alias   | sysadmin | Yes    |
| become live                                           |          |        |
+-------------------------------------------------------+----------+--------+
| Add a LastPass account and add to relevant LastPass   | sysadmin | Yes    |
| group                                                 |          |        |
+-------------------------------------------------------+----------+--------+
| Send Slack invitation                                 | sysadmin | Yes    |
+-------------------------------------------------------+----------+--------+
| Send a Toggl invitation                               | HR       | Yes    |
+-------------------------------------------------------+----------+--------+
| Send a Xero invitation                                | HR       | Yes    |
+-------------------------------------------------------+----------+--------+
| Send TurbineHQ invitation                             | HR       | Yes    |
+-------------------------------------------------------+----------+--------+
| Create new employee for Payroll                       | HR       | Yes    |
+-------------------------------------------------------+----------+--------+
| Create Omnislip login ID for Payroll User             | HR       | Yes    |
+-------------------------------------------------------+----------+--------+
| Setup Google Drive with the right folders             | employee | Yes    |
+-------------------------------------------------------+----------+--------+
| Add contact information to Contact spreadsheet        | employee | Yes    |
+-------------------------------------------------------+----------+--------+
| Email Rufus or Fiona for adding into Know Your        | sysadmin | Yes    |
| Company                                               |          |        |
+-------------------------------------------------------+----------+--------+
| Send introuction email to all-staff@                  | manager  | Yes    |
+-------------------------------------------------------+----------+--------+
| Add to relevant Mailman lists                         | manager  | No     |
+-------------------------------------------------------+----------+--------+
| Github owners team access (only if needed)            | manager  | No     |
+-------------------------------------------------------+----------+--------+
| Add to RT                                             | sysadmin | No     |
+-------------------------------------------------------+----------+--------+
| Access to blogfarm                                    | sysadmin | No     |
|                                                       | or       |        |
|                                                       | manager  |        |
+-------------------------------------------------------+----------+--------+
| Add keys to infra repo and grant access to servers    | sysadmin | No     |
+-------------------------------------------------------+----------+--------+
| Access to Highrise                                    | HR       | No     |
+-------------------------------------------------------+----------+--------+
| Calliflower                                           | HR       | No     |
+-------------------------------------------------------+----------+--------+
| Email Neal to add to Team page on website             | manager  | No     |
+-------------------------------------------------------+----------+--------+


Leavers
-------

+-------------------------------------------------------+----------+
| Action                                                | Team     |
+=======================================================+==========+
| Inform Employee that their access to accounts will be | Manager  |
| suspended                                             |          |
+-------------------------------------------------------+----------+
| Suspend Google Apps account and transfer docs         | sysadmin |
+-------------------------------------------------------+----------+
| Remove from all Google Groups                         | sysadmin |
+-------------------------------------------------------+----------+
| Remove from Toggl                                     | HR       |
+-------------------------------------------------------+----------+
| Remove from Xero                                      | HR       |
+-------------------------------------------------------+----------+
| Remove contact information from Contact spreadsheet   | HR       |
+-------------------------------------------------------+----------+
| Remove account from Highrise                          | HR       |
+-------------------------------------------------------+----------+
| Remove account from Basecamp                          | HR       |
+-------------------------------------------------------+----------+
| Calliflower                                           | HR       |
+-------------------------------------------------------+----------+
| Remove from TurbineHQ                                 | HR       |
+-------------------------------------------------------+----------+
| Email Neal to remove from Team page on website        | manager  |
+-------------------------------------------------------+----------+
| Remove from Slack                                     | sysadmin |
+-------------------------------------------------------+----------+
| Remove from LastPass                                  | sysadmin |
+-------------------------------------------------------+----------+
| Remove from any aliases on FastMail                   | sysadmin |
+-------------------------------------------------------+----------+
| Remove from Github owners team                        | sysadmin |
+-------------------------------------------------------+----------+
| Remove from Dropbox                                   | sysadmin |
+-------------------------------------------------------+----------+
| Remove access to all blogfarm sites. Do not delete    | sysadmin |
| user. Downgrade to contributor on all sites           |          |
+-------------------------------------------------------+----------+
| Remove keys from infra repo and access to servers     | sysadmin |
+-------------------------------------------------------+----------+
| Remove from Mailman lists                             | sysadmin |
| (`remove_members --fromall`)                          |          |
+-------------------------------------------------------+----------+
| Remove from RT                                        | sysadmin |
+-------------------------------------------------------+----------+
| Email Rufus, or Fiona for removing from Know Your     | sysadmin |
| Company                                               |          |
+-------------------------------------------------------+----------+

Managing Leavers Emails
-----------------------
Email Redirects and Auto-Responders
-----------------------------------
There are three main ways we can deal with emails once someone has left.  We
default to option 1, which is to disable the account and redirect email to
a generic auto-response.  We normally just disable the account initially and
after a couple of months, the account is removed totally.  Once the account is
removed, the emails are gone forever.  If you want either of the two other
options, please ask.

- Option 1: We just disable the account and direct emails to a generic
  auto-response.
- Option 2: We can setup a auto-response specific to that person.  Please send
  the sysadmin team the full text of the auto-response and how long you want it
  for up to a maximum of 6 months.
- Option 3: We can redirect all emails from the user to another OK Staffer.
  Please inform the staffer that you've requested this for them.

Access to leavers emails
------------------------
We have Google Vault which gives us extensive access to search through the
emails of users.  We can either leave a users account on the system so we can
use google vault to search their emails.  Or we can export their emails to an
mbox email file which can be searched through for emails manually.  We default
to exporting and storing the users emails for one year.  After a year, we
delete the emails.  So options are:

 - Option 1 (default) - Emails are exported into a mbox file and stored on
   Google Drive.
 - Option 2 - The user is suspended and you can use Google Vault.  This costs
   money and you'll need to tell us when to remove the user from the system
   (and what you want done with their email then).
